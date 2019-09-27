# CircularProgress

CircularProgress/圆形进度条/环形进度条/vue/weex 插件/纯 css 写法

## 编者语

简单的小插件，欢迎拷贝走，记得给个 star！后续可能有更新！

本插件是在 weex 项目过程中开发的，开始的时候有很多坑，后面逐步完善起来，个人觉得这个插件还不错，可以分享一下，于是用 vue 脚手架搭建重新整理了一份用例，以上。

## 引用写法

按照普通组件引用即可

```vue
<template>
  <!-- ... -->
  <CircularProgress auto />
  <!-- ... -->
</template>
<script>
import CircularProgress from './CircularProgress';
export default {
  components: {
    CircularProgress
  }
  // ...
};
</script>
```

## 参数

<table>
  <tr>
    <th>参数</th>
    <th>参数名称</th>
    <th>参数描述</th>
    <th>默认值</th>
    <th>需求参数类型</th>
  </tr>
  <tr>
    <td>rate</td>
    <td>当前进度</td>
    <td>范围(0-1)，使用时，需要用当前值除以总值得到。当启动自动进度时，该参数失效</td>
    <td>0</td>
    <td>Number</td>
  </tr>
  <tr>
    <td>width</td>
    <td>进度条直径</td>
    <td>使用该进度条时，需要在圈外留足空间，不然会出现错位</td>
    <td>500</td>
    <td>Number/String</td>
  </tr>
  <tr>
    <td>borderWidth</td>
    <td>进度条粗细</td>
    <td></td>
    <td>8</td>
    <td>Number/String</td>
  </tr>
  <tr>
    <td>borderColor</td>
    <td>进度条颜色</td>
    <td></td>
    <td>#fff</td>
    <td>String</td>
  </tr>
  <tr>
    <td>innerWidth</td>
    <td>衬托圈直径</td>
    <td>使用内容时，以进度条圈范围为准，衬托圈只作为装饰使用。不传代表自动计算放在进度条圈的正中间</td>
    <td>0</td>
    <td>Number/String</td>
  </tr>
  <tr>
    <td>innerBorderWidth</td>
    <td>衬托圈粗细</td>
    <td></td>
    <td>2</td>
    <td>Number/String</td>
  </tr>
  <tr>
    <td>innerBorderColor</td>
    <td>衬托圈颜色</td>
    <td></td>
    <td>#fff</td>
    <td>String</td>
  </tr>
  <tr>
    <td>doubleProgress</td>
    <td>是否双层进度</td>
    <td>当进度大于1小于2的时候是否显示进度大于1。进度超过1时，进度条效果改为消失比例</td>
    <td>false</td>
    <td>Boolean</td>
  </tr>
  <tr>
    <td>auto</td>
    <td>自动进度</td>
    <td>用于当前无法确定进度但又有动画进度需求时，进度条默认走两圈后重置，每两圈间隔1秒</td>
    <td>false</td>
    <td>Boolean</td>
  </tr>
  <tr>
    <td>autoSpeed</td>
    <td>自动速度</td>
    <td>自动进度时配置的参数。默认8秒完成半圈，数字越小速度越快。建议使用1/2/4/8/16，不然在html中半圆的首尾相接会出现断位</td>
    <td>8</td
    ><td>Number</td>
  </tr>
  <tr>
    <td>point</td>
    <td>是否显示顶点</td>
    <td></td>
    <td>false</td>
    <td>Boolean</td>
  </tr>
</table>
<table>
  <tr>
    <th>插槽</th>
    <th>插槽名称</th>
    <th>插槽描述</th>
  </tr>
  <tr>
    <td>--</td>
    <td>默认插槽</td>
    <td>往环形进度条中间添加内容</td>
  </tr>
  <tr>
    <td>point</td>
    <td>顶点插槽</td>
    <td>替换进度条顶点图标，需要注意大小超过50*50px时你需要修改源码，不超过时，将插件中`.progress-point-img`的内容复制出来做修改</td>
  </tr>
</table>

## 运行 demo 命令

`vue serve`

## 用例目录

1. 自动旋转
2. 1+进度顶点
3. 2+改变颜色和大小
4. 3+内容
5. 4+改变自动旋转速度
6. 1+改变进度顶点图案
7. 6+使用图片
8. 静止在某个百分比
9. 8+调节百分比

## 做这个插件的时候踩过的 weex 的坑

1. css 属性不能连写，需要拆的很开（日常坑）
2. html 模板上的计算样式:style 的内容，如果在 css 中有默认的内容，会出现闪屏问题，会先重置为 css 默认样式，然后用计算样式覆盖
3. 在 Android 环境下，单独设置单一个边框(例如:`border-top`)时，他与另一个边框的交汇位置不是直线，而是你单独设置的边框，这会导致半个正方形遮住了整个半圆，会上下都有点突出来
4. weex 下的所有 div 默认是 `display:flex;box-sizing: border-box;`  
5.在数据改变时，计算样式:style 的内容 "transform: `translateY(-${(realWidth-borderWidth)/2}px) rotate(${angle}deg) `"，有可能有半句失效，平移旋转3D变换连写有风险

## 参考

1. [vue 快速原型开发](https://cli.vuejs.org/zh/guide/prototyping.htm1l)
2. [flex 布局-菜鸟教程](https://www.runoob.com/w3cnote/flex-grammar.html)
3. [weex 官网-css-transition](https://weex.apache.org/zh/docs/styles/common-styles.html#transition)

### 已知问题

1. 当进度从小于 0.5 跳跃至大于 0.5 时，进度条可能会有断位
