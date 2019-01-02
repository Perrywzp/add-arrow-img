<template>
  <div id="app">
    <div style="float: left; margin-left: 120px;">
      <p>经增加了箭头数据的图片, 进行了比例缩放处理，右侧是原图</p>
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
    </div>
    <div style="float: right; width: 1200px;">
      <img v-for="(item, index) in mocks" :src="item.src" :key="index">
    </div>
  </div>

</template>
<script>
import AddArrowImg from './libs/add-arrow-img/main.vue'

export default {
  name: 'app',
  components: {
    AddArrowImg
  },
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
      },
      mocks: []
    }
  },
  mounted () {
    this.round()
  },
  methods: {
    round () {
      this.mocks = [
        // {
        //   src: 'https://cdn.pixabay.com/photo/2018/09/30/19/03/railway-station-3714297__340.jpg',
        //   drawType: 1,
        //   line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
        //   polyLine: [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }],
        //   direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.4, "y": 0.5 } }
        // },
        // {
        //   src: 'https://cdn.pixabay.com/photo/2018/11/15/22/55/wolf-3818354__340.jpg',
        //   drawType: 1,
        //   line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
        //   polyLine: [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }],
        //   direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } }
        // },
        // {
        //   src: 'https://cdn.pixabay.com/photo/2018/11/29/19/29/autumn-3846345__340.jpg',
        //   drawType: 2,
        //   line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
        //   polyLine: [{ "x": 0.2, "y": 0.4 }, { "x": 0.3, "y": 0.3 }, { "x": 0.4, "y": 0.4 }, { "x": 0.6, "y": 0.2 }],
        //   direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } }
        // }
        {
          src: require('@/assets/imgs/montain.jpg'),
          drawType: 1,
          line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
          polyLine: [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }],
          direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.4, "y": 0.5 } }
        },
        {
          src: require('@/assets/imgs/wolf.jpg'),
          drawType: 1,
          line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
          polyLine: [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }],
          direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } }
        },
        // {
        //   src: require('@/assets/imgs/summer.jpg'),
        //   drawType: 2,
        //   line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
        //   polyLine: [{ "x": 0.2, "y": 0.4 }, { "x": 0.3, "y": 0.3 }, { "x": 0.4, "y": 0.4 }, { "x": 0.6, "y": 0.2 }],
        //   direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } }
        // },
        {
          src: require('@/assets/imgs/house.jpg'),
          drawType: 1,
          line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
          polyLine: [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }],
          direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } }
        },
        {
          src: require('@/assets/imgs/dark.jpg'),
          drawType: 1,
          line: { "StartPoint": { "x": 0.5, "y": 0.1 }, "EndPoint": { "x": 0.5, "y": 0.9 } },
          polyLine: [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }],
          direction: { "StartPoint": { "x": 0.5, "y": 0.5 }, "EndPoint": { "x": 0.6, "y": 0.5 } }
        }
      ]
      setInterval(()=> {
        this.data = this.mocks[Math.floor(Math.random() * 5)]
      }, 2000)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
