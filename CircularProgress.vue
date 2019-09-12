<template>
  <div class="progress-wrapper">
    <div class="progress-all-circle"
      :style="{
        width: wrapperWidth + 'px',
        height: wrapperWidth + 'px',
        margin: wrapperPadding + 'px',
      }">
      <div class="circle-inner"
        :style="{
          width: realInnerWidth + 'px',
          height: realInnerWidth + 'px',
          left: ((wrapperWidth-realInnerWidth) / 2) + 'px',
          top: ((wrapperWidth-realInnerWidth) / 2) + 'px',
          border: `${innerBorderWidth}px ${innerBorderColor} solid`,
          'border-radius': realInnerWidth + 'px',
        }">
      </div>
      <div class="circle-progress-first-half"
        :style="{
          width: (wrapperWidth / 2) + 'px',
          height: wrapperWidth + 'px',
        }">
        <div v-if="refreshed"
          class="progress-bar-first-half"
          :style="{
            width: realWidth + 'px',
            height: realWidth + 'px',
            top:  ((wrapperWidth-realWidth)/2) + 'px',
            right:  ((wrapperWidth-realWidth)/2) + 'px',
            'border-width': borderWidth + 'px',
            'border-radius': realWidth + 'px',
            'border-bottom-color': borderColor,
            'border-left-color': borderColor,
            transform: `rotate(${45+firstHalfAngle}deg)`,
          }"
        ></div>
      </div>
      <div class="circle-progress-last-half"
        :style="{
          width: (wrapperWidth / 2) + 'px',
          height: wrapperWidth + 'px',
        }">
        <div v-if="refreshed"
          class="progress-bar-last-half"
          :style="{
            width: realWidth + 'px',
            height: realWidth + 'px',
            top:  ((wrapperWidth-realWidth)/2) + 'px',
            left:  ((wrapperWidth-realWidth)/2) + 'px',
            'border-width': borderWidth + 'px',
            'border-radius': realWidth + 'px',
            'border-bottom-color': borderColor,
            'border-left-color': borderColor,
            transform: `rotate(${45+lastHalfAngle}deg)`,
          }"
        ></div>
      </div>
      <!-- 这里旋转，宽高不是奇数，所以转起来会有1个像素的偏差 -->
      <div v-if="point&&refreshed"
        class="progress-point-content"
        :style="{
            top:  (wrapperWidth/2) + 'px',
            left:  (wrapperWidth/2) + 'px',
            'transform-origin': `25px ${(realWidth-borderWidth)/2+25}px`,
            transform: `translateY(-${(realWidth-borderWidth)/2}px) rotate(${angle}deg) `
        }">
        <slot name="point">
          <div class="progress-point-img" :style="{
            'background-color': borderColor
          }"></div>
        </slot>
      </div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    rate: {
      type: Number, // 当前进度/总进度
      default: 0
    },
    width: {
      // 外圈大小
      type: [Number, String],
      default: 500
    },
    borderWidth: {
      // 外圈border宽度（粗细）
      type: [Number, String],
      default: 8
    },
    borderColor: {
      // 外圈border颜色
      type: String,
      default: "#fff"
    },
    innerWidth: {
      // 内圈大小（不传代表自动计算）
      type: [Number, String],
      default: 0
    },
    innerBorderWidth: {
      // 内圈border宽度（粗细）
      type: [Number, String],
      default: 2
    },
    innerBorderColor: {
      // 内圈border颜色
      type: String,
      default: "#fff"
    },
    doubleProgress: Boolean, // 当进度大于1小于2的时候是否显示进度大于1
    auto: Boolean, // 增加参数控制是否自动转圈圈
    autoSpeed: { // 自动转圈速度，默认8秒完成半圈，数字越小，速度越快，不能为0。最好用1/2/4/8/16，不然在html中对接末尾的首尾相继会出现断位
      type: Number,
      default: 8
    },
    point: Boolean, // 是否显示定点图标
  },
  data() {
    return {
      wrapperWidth: 0, // 容器大小
      wrapperPadding: 0,
      realWidth: 0, // 处理后真实外圈宽度（包含边框）
      realInnerWidth: 0, // 处理后真实内圈宽度（包含边框）
      refreshed: true,
      autoRate: 0,
      autoTimer: null,
    };
  },
  computed: {
    angle() { // 进度圆点位置
      const rate = this.auto ? this.autoRate : this.rate;
      if (rate < 0) return 0;
      if (rate > 2) return 720;
      return Math.floor(360 * rate);
    },
    firstHalfAngle() {
      const rate = this.auto ? this.autoRate : this.rate;
      if (rate <= 0) return 0;
      if (rate > 0 && rate <= 0.5) return Math.floor(360 * rate);
      if (rate > 0.5 && rate <= 1) return 180;
      if (!(this.auto || this.doubleProgress) && rate > 1) return 180;
      if ((this.auto || this.doubleProgress) && rate > 1 && rate <= 1.5) return 180 + Math.floor(360 * (rate - 1));
      if ((this.auto || this.doubleProgress) && rate > 1.5 && rate <= 2) return 360;
      if (rate > 2) return 360;
    },
    lastHalfAngle() {
      const rate = this.auto ? this.autoRate : this.rate;
      if (rate <= 0) return 180;
      if (rate > 0 && rate <= 0.5) return 180;
      if (rate > 0.5 && rate <= 1) return Math.floor(360 * rate);
      if (!(this.auto || this.doubleProgress) && rate > 1) return 360;
      if ((this.auto || this.doubleProgress) && rate > 1 && rate <= 1.5) return 360;
      if ((this.auto || this.doubleProgress) && rate > 1.5 && rate <= 2) return 360 + Math.floor(360 * (rate - 1.5));
      if (rate > 2) return 360 + 180;
    },
  },
  methods: {
    refresh(fn) {
      this.refreshed = false;
      setTimeout(() => {
        fn && fn();
        this.refreshed = true
      }, 20);
    }
  },
  created() {
    this.wrapperWidth = Math.floor((parseInt(this.width) + 40) / 2) * 2;
    this.realWidth = Math.floor(parseInt(this.width) / 2) * 2;
    this.realInnerWidth = this.innerWidth
      ? Math.floor(parseInt(this.innerWidth) / 2) * 2
      : this.realWidth - (parseInt(this.borderWidth) - parseInt(this.innerBorderWidth));
    this.wrapperPadding = (this.wrapperWidth - this.realInnerWidth) / 2;
  },
  mounted() {
    if (this.auto) {
      this.autoTimer = setInterval(() => {
          if(this.autoRate > 2) {
              this.refresh(() => {
                  this.autoRate = 0;
              })
          }
          this.autoRate += 0.5 / (this.autoSpeed || 8); // x秒完成半圈
      }, 1000)
    }
  },
  beforeDestroy() {
    if (this.autoTimer) clearInterval(this.autoTimer);
  }
};
</script>

<style>
/* 以下为原版样式，删掉:style后，开启注释内容，可以正常运行 */
.progress-wrapper {
  display: flex;
  align-items: center;
  overflow: hidden;

}
.progress-all-circle {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  /* width: 640px;
  height: 640px; */
}
.circle-inner {
  display: flex;
  position: absolute;
  box-sizing: border-box;
  /* border-width: 2px;
  border-color: #fff;
  width: 596px;
  height: 596px;
  left: 22px;
  top: 22px;
  border-radius: 596px; */
  justify-content: center;
  align-items: center;
}
.circle-progress-first-half,
.circle-progress-last-half {
  box-sizing: border-box;
  display: flex;
  position: absolute;
  overflow: hidden;
  top: 0;
  /* width: 320px;
  height: 640px;
  left: 320px; */
}
.circle-progress-first-half{
  right: 0;
}
.circle-progress-last-half {
  left: 0;
}
.progress-bar-first-half,
.progress-bar-last-half {
  box-sizing: border-box;
  display: flex;
  position: absolute;
  border-top-color: transparent;
  border-right-color: transparent;
  transition: transform 1s linear;
  border-style: solid;
  /* border-width: 6px;
  width: 600px;
  height: 600px;
  top: 320px;
  border-radius: 600px;
  border-left-color: #b4653d;
  border-bottom-color: #b4653d; */
}
.progress-bar-first-half {
  transform-origin: center;
  transform: rotate(45deg);
  /* 旋转角度angle+45 */
}
.progress-bar-last-half {
  transform-origin: center;
  transform: rotate(225deg);
  /* 旋转角度angle+45（angle最低180） */
}

.progress-point-content{
  box-sizing: border-box;
  display: flex;
  position: absolute;
  width: 50px;
  height: 50px;
  margin-left: -25px;
  margin-top: -25px;
  transition: transform 1s linear;
}
.progress-point-img{
  /* 如果要重写进度条顶点，把这段复制出去再修改 */
  position: absolute;
  left: 25px;
  top: 25px;
  width: 20px;
  height: 20px;
  margin-left: -10px;
  margin-top: -10px;
  border-radius: 100px;
  /* background-color: #fff; */
}
</style>
