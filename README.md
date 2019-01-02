## add-arrow-img 
![示例1](/src/assets/demos/cap.png)
![示例2](/src/assets/demos/cap2.png)
## Install
```shell
npm install add-arrow-img -S
```

## Quick Start
``` javascript
import Vue from 'vue'
import AddArrowImg from 'add-arrow-img'

Vue.use(AddArrowImg)
```

## .vue文件使用标签
``` javascript
<template>
  <add-arrow-img
  :width="layout.width"
  :height="layout.height"
  :src="data.src"
  :drawType="data.drawType"
  :line="data.line"
  :polyLine="data.polyLine"
  :direction="data.direction"
  :lineColor="lineColor"
  :lineWidth="lineWidth"
  ></add-arrow-img>
</template>
<script>
export default {
  name: 'app',
  data () {
    return {
      lineColor: '#ff4400',
      lineWidth: 3,
      layout: {width: 480, height: 270},
      data: {
        src: require('@/assets/imgs/house.jpg'),
        drawType: 1,
        line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
        polyLine: [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }],
        direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } }
      }
    }
  }
}
</script>
```

### LedScreenSet Attributes
| 参数         | 说明            | 类型            | 可选值                 | 默认值   |
|------------- |---------------- |---------------- |---------------------- |-------- |
| width       | 布局宽 | Number  | — | 640 |
| height       | 布局高 | Number  | — | 320 |
| src     | 渲染原图资源 | String | — | — |
| lineColor | 画线颜色 | String  |    —  |  '#ff4400' |
| drawType | 绘制类型 1 直线， 2 折线 | Number  |    —  |  1 |
| polyLine| 折线数据点位 如： [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }] | Array  |    —  |  —  |
| direction | 箭头指向数据 如：{ "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } } | Object  |    —  |  —  |
| line | 直线数据 如： { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } } | Object  |    —  |  —  |
## Browser Support
Modern browsers and Internet Explorer 10+.

