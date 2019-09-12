# CircularProgress
CircularProgress/圆形进度条/环形进度条/vue/weex插件/纯css写法

## 编者语
简单的小插件，欢迎拷贝走，记得给个star！后续可能有更新！

本插件是在weex项目过程中开发的，开始的时候有很多坑，后面逐步完善起来，个人觉得这个插件还不错，可以分享一下，于是用vue脚手架搭建重新整理了一份用例，以上。

## 引用写法

按照普通组件引用即可
```vue
<template>
  <!-- ... -->
  <CircularProgress auto/>
  <!-- ... -->
</template>
<script>
import CircularProgress from './CircularProgress';
export default {
  components: {
    CircularProgress
  },
  // ...
}
</script>
```

## 参数

|参数|参数名称|参数描述|默认值|需求参数类型|
|-|-|-|-|-|-|-|
|rate|当前进度|范围(0-1)，使用时，需要用当前值除以总值得到。当启动自动进度时，该参数失效|0|Number|
|width|进度条直径|使用该进度条时，需要在圈外留足空间，不然会出现错位|500|Number/String|
|borderWidth|进度条粗细||8|Number/String|
|borderColor|进度条颜色||#fff|String|
|innerWidth|衬托圈直径|使用内容时，以进度条圈范围为准，衬托圈只作为装饰使用。不传代表自动计算放在进度条圈的正中间|0|Number/String|
|innerBorderWidth|衬托圈粗细||2|Number/String|
|innerBorderColor|衬托圈颜色||#fff|String|
|doubleProgress|是否双层进度|当进度大于1小于2的时候是否显示进度大于1。进度超过1时，进度条效果改为消失比例|false|Boolean|
|auto|自动进度|用于当前无法确定进度但又有动画进度需求时，进度条默认走两圈后重置，每两圈间隔1秒|false|Boolean|
|autoSpeed|自动速度|自动进度时配置的参数。默认8秒完成半圈，数字越小速度越快。建议使用1/2/4/8/16，不然在html中半圆的首尾相接会出现断位|8|Number|
|point|是否显示顶点||false|Boolean|

## 运行demo命令

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

## 做这个插件的时候踩过的weex的坑
1. css属性不能连写，需要拆的很开（日常坑）
2. html模板上的计算样式:style的内容，如果在css中有默认的内容，会出现闪屏问题，会先重置为css默认样式，然后用计算样式覆盖
3. 在Android环境下，单独设置单一个边框(例如:border-top)时，他与另一个边框的交汇位置不是直线，而是你单独设置的边框，这会导致半个正方形遮住了整个半圆，会上下都有点突出来
4. weex下的所有div默认是display:flex;box-sizing: border-box;

## 参考

1. [vue快速原型开发](https://cli.vuejs.org/zh/guide/prototyping.htm1l)
2. [flex布局-菜鸟教程](https://www.runoob.com/w3cnote/flex-grammar.html)
3. [weex官网-css-transition](https://weex.apache.org/zh/docs/styles/common-styles.html#transition)

# 已知问题
1. 当进度从小于0.5跳跃至大于0.5时，进度条可能会有断位
