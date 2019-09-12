<template>
  <div class="app">
    <div class="demo" v-if="demo[0].show">
      <div class="demo-title">{{demo[0].title}}</div>
      <div class="demo-desc">{{demo[0].desc}}</div>
      <div class="demo-content">
        <CircularProgress auto/>
      </div>
    </div>
    <div class="demo" v-if="demo[1].show">
      <div class="demo-title">{{demo[1].title}}</div>
      <div class="demo-desc">{{demo[1].desc}}</div>
      <div class="demo-content">
        <CircularProgress auto point/>
      </div>
    </div>
    <div class="demo" v-if="demo[2].show">
      <div class="demo-title">{{demo[2].title}}</div>
      <div class="demo-desc">{{demo[2].desc}}</div>
      <div class="demo-content">
        <CircularProgress auto point borderColor="red" innerBorderWidth="8" innerWidth="450px"/>
      </div>
    </div>
    <div class="demo" v-if="demo[3].show">
      <div class="demo-title">{{demo[3].title}}</div>
      <div class="demo-desc">{{demo[3].desc}}</div>
      <div class="demo-content">
        <CircularProgress auto point borderColor="red" innerBorderWidth="8" innerWidth="450px">
          <div>花里胡哨的内容</div>
        </CircularProgress>
      </div>
    </div>
    <div class="demo" v-if="demo[4].show">
      <div class="demo-title">{{demo[4].title}}</div>
      <div class="demo-desc">{{demo[4].desc}}</div>
      <div class="demo-content">
        <CircularProgress auto :autoSpeed="1" point borderColor="red" innerBorderWidth="8" innerWidth="450px">
          <div>花里胡哨的内容</div>
        </CircularProgress>
      </div>
    </div>
    <div class="demo" v-if="demo[5].show">
      <div class="demo-title">{{demo[5].title}}</div>
      <div class="demo-desc">{{demo[5].desc}}</div>
      <div class="demo-content">
        <CircularProgress auto point>
          <div slot="point" class="my-point"></div>
        </CircularProgress>
      </div>
    </div>
    <div class="demo" v-if="demo[6].show">
      <div class="demo-title">{{demo[6].title}}</div>
      <div class="demo-desc">{{demo[6].desc}}</div>
      <div class="demo-content">
        <CircularProgress auto point>
          <div slot="point" class="my-img"></div>
        </CircularProgress>
      </div>
    </div>
    <div class="demo" v-if="demo[7].show">
      <div class="demo-title">{{demo[7].title}}</div>
      <div class="demo-desc">{{demo[7].desc}}</div>
      <div class="demo-content">
        <CircularProgress :rate="60/100"/>
      </div>
    </div>
    <div class="demo" v-if="demo[8].show">
      <div class="demo-title">{{demo[8].title}}</div>
      <div class="demo-desc">{{demo[8].desc}}</div>
      <div class="demo-desc"><input v-model="demo[8].number" type="number"></div>
      <div class="demo-content">
        <CircularProgress :rate="demo[8].number/100"/>
      </div>
    </div>
  </div>
</template>

<script>
import CircularProgress from './CircularProgress';
export default {
  components: {
    CircularProgress
  },
  data() {
    return {
      demo: [
        {title: '1.自动旋转', desc: '', show: true},
        {title: '2.1+进度顶点', desc: '', show: false},
        {title: '3.2+改变颜色和大小', desc: '注意留足位置，不然进度条会变形', show: false},
        {title: '4.3+内容', desc: '', show: false},
        {title: '5.4+改变自动旋转速度', desc: '', show: false},
        {title: '6.1+改变进度顶点图案', desc: '', show: false},
        {title: '7.6+使用图片', desc: '', show: false},
        {title: '8.静止在某个百分比', desc: '', show: false},
        {title: '9.8+调节百分比', desc: '', show: false, number: 10},
      ]
    }
  }
}
</script>

<style>
html, body {
  width: 100%;
  height: 100%;
}
body {
  overflow: auto;
  margin: 0;
}
.app {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
.demo{
  width: 750px;
  height: 750px;
  margin: 10px;
}
.demo-content {
  background-color: #999;
}
.my-point{
  background-color: yellow;
  /* 如果要重写进度条顶点，把这段复制出去再修改 */
  position: absolute;
  left: 25px;
  top: 25px;
  width: 40px;
  height: 40px;
  margin-left: -20px;
  margin-top: -20px;
}
.my-img{
  position: absolute;
  left: 25px;
  top: 25px;
  width: 40px;
  height: 40px;
  margin-left: -20px;
  margin-top: -20px;
  background: url('./img-point.png') no-repeat center;
  background-size: contain;
}
</style>
