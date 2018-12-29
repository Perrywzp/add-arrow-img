<template>
  <canvas ref="canvas">
    Your browser does not support the HTML5 canvas tag.
  </canvas>
</template>
<script>
export default {
  name: 'add-arrow-img',
  props: {
    src: {
      type: String,
      default: 'https://cdn.pixabay.com/photo/2018/07/08/01/44/mountains-3523153__340.jpg'
    },
    drawType: {
      type: Number,
      default: 1 // 1直线， 2折线
    },
    // 绘制线颜色
    lineColor:{
      type: String,
      default: '#ff4400'
    },
    // 绘制线宽度
    lineWidth: {
      type: String,
      default: '3'
    },
    // 直线数据
    line: {
      type: Object,
      default () {
        return {
          "StartPoint": { "x": 0.1, "y": 0.1 },
          "EndPoint": { "x": 0.2, "y": 0.2 }
        }
      }
    },
     // 折线数据
    polyLine: {
      type: Array,
      default () {
        return [{ "x": 0.1, "y": 0.1 }, { "x": 0.2, "y": 0.2 }, { "x": 0.3, "y": 0.1 }, { "x": 0.4, "y": 0.3 }]
      }
    },
    // 箭头数据
    direction: {
      type: Object,
      default () {
        return {
          "StartPoint": { "x": 0.1, "y": 0.1 },
          "EndPoint": { "x": 0.2, "y": 0.3 }
        }
      }
    }
  },
  data() {
    return {
      ctx: '',
      imgSize: {
        width: 40,
        height: 40
      },
      creatCanvas: false,
      img: ''
    }
  },
  watch: {
    src (val) {
      this.creatCanvas = false
      this.loadImg(val)
    },
    creatCanvas (val) {
      if(val) {
        this.render()
      }
    }
  },
  mounted() {
    this.loadImg(this.src)
  },
  methods: {
    // 绘制图
    render () {
      let canvas = this.$refs.canvas
      this.ctx = canvas.getContext("2d")
      canvas.width = this.imgSize.width
      canvas.height = this.imgSize.height
      this.drawImg(this.img)
      if (this.drawType === 1) {
        this.drawLine(this.ctx, this.line.StartPoint, this.line.EndPoint, this.imgSize, this.lineWidth, this.lineColor)
        let {width, height} = this.imgSize
         let start = this.direction.StartPoint
        let end = this.direction.EndPoint
        start = { x: start.x * width, y: start.y * height}
        end = { x: end.x * width, y: end.y * height}
        this.drawArrow(this.ctx, start.x, start.y, end.x, end.y, 30, 10, this.lineWidth, this.lineColor, false, true)
      } 
      if (this.drawType === 2) {
        this.drawPolyLine(this.ctx, this.polyLine, this.imgSize, this.lineWidth, this.lineColor)
        let {width, height} = this.imgSize
        let start = this.direction.StartPoint
        let end = this.direction.EndPoint
        start = { x: start.x * width, y: start.y * height}
        end = { x: end.x * width, y: end.y * height}
        /* eslint-disable */
        console.log(start, end)
        this.drawArrow(this.ctx, start.x, start.y, end.x, end.y, 30, 10, this.lineWidth, this.lineColor, false, true)
      }
      return this
    },
    
    // 上图
    drawImg(img) {
      if(!this.ctx) return
      this.ctx.drawImage(img, 0, 0, img.width, img.height)
      return this
    },
    // 绘制直线
    drawLine (ctx, start, end, imgSize, width, color) {
      ctx.save()
      ctx.beginPath()
      ctx.lineCap="round"
      ctx.lineJoin="round"
      ctx.moveTo(start.x * imgSize.width, start.y * imgSize.height)
      ctx.lineTo(end.x * imgSize.width, end.y * imgSize.height)
      ctx.lineWidth = width
      ctx.strokeStyle = color
      ctx.stroke()
      ctx.closePath()
    },
    // 绘制折线
    drawPolyLine(ctx, polyLine, imgSize, width, color) {
      ctx.save()
      ctx.beginPath()
      ctx.lineCap="round"
      ctx.lineJoin="round"
      polyLine.forEach((item,index) => {
        if( index < polyLine.length - 1){
          ctx.moveTo(item.x * imgSize.width, item.y * imgSize.height)
          ctx.lineTo(polyLine[index + 1].x * imgSize.width, polyLine[index + 1].y * imgSize.height)
        }
      })
      ctx.lineWidth = width
      ctx.strokeStyle = color
      ctx.stroke()
      ctx.closePath()
    },
    // 加载图片
    loadImg (src) {
      let img = this.img = document.createElement('img')
      img.src = src
      img.onload = () => {
        this.imgSize = { 
          width: img.width,
          height: img.height
        }
        this.creatCanvas = true
      }
    },
    // 绘制箭头
    drawArrow (ctx, fromX, fromY, toX, toY, theta, headlen, width, color, startArrow, endArrow) { 
      theta = typeof(theta) !== 'undefined' ? theta : 30
      headlen = typeof(theta) !== 'undefined' ? headlen : 10
      width = typeof(width) !== 'undefined' ? width : 1
     
      let angle = Math.atan2(fromY - toY, fromX - toX) * 180 / Math.PI
      let angle1 = (angle + theta) * Math.PI / 180
      let angle2 = (angle - theta) * Math.PI / 180
      let topX = headlen * Math.cos(angle1)
      let topY = headlen * Math.sin(angle1)
      let botX = headlen * Math.cos(angle2)
      let botY = headlen * Math.sin(angle2)
      ctx.save()
      ctx.beginPath() 
      let arrowX = fromX - topX
      let arrowY = fromY - topY
      // 绘制开始箭头
      if (startArrow) {
        ctx.moveTo(arrowX, arrowY) 
        ctx.lineTo(fromX, fromY) 
        arrowX = fromX - botX 
        arrowY = fromY - botY 
        ctx.lineTo(arrowX, arrowY) 
      }
      ctx.moveTo(fromX, fromY) 
      ctx.lineTo(toX, toY) 
      // 绘制结束箭头
      if (endArrow) {
      // Reverse length on the other side 
        arrowX = toX + topX 
        arrowY = toY + topY 
        ctx.moveTo(arrowX, arrowY)
        ctx.lineTo(toX, toY) 
        arrowX = toX + botX
        arrowY = toY + botY 
        ctx.lineTo(arrowX, arrowY) 
      }
      ctx.strokeStyle = color 
      ctx.lineWidth = width 
      ctx.stroke() 
      ctx.restore()
     }
  }
}
</script>