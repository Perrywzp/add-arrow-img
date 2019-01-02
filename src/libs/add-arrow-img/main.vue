<template>
  <div class="arrow-img-box" :style="{width: width + 'px', height: height + 'px' , background: background}">
    <img :src="compositeImage" :style="{width: imgSize.width + 'px', height: imgSize.height + 'px'}">
  </div>
</template>
<script>
export default {
  name: 'add-arrow-img',
  props: {
    // 默认按16:9的比例设置
    width: {
      type: Number,
      default: 640
    },
    height: {
      type: Number,
      default: 320
    },
    background: {
      type: String,
      default: '#333'
    },
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
      compositeImage: '',
      imgSize: {
        width: 40,
        height: 40
      },
      img: ''
    }
  },
  watch: {
    src (val) {
      this.loadImg(val)
    }
  },
  mounted() {
    this.loadImg(this.src)
  },
  methods: {
    // 加载图片
    loadImg (src) {
      let img = this.img = document.createElement('img')
      img.src = src
      img.setAttribute("crossOrigin",'Anonymous')
      img.onload = () => {
        this.compositeImage = this.getBase64(img)
        this.imgSize = this.calcRenderSize(this.width, this.height, img.width, img.height)
      }
    },
    // 绘制图
    getBase64 (img) {
      let canvas = document.createElement('canvas')
      let ctx = canvas.getContext("2d")
      canvas.width = img.width
      canvas.height = img.height
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height)
      if (this.drawType === 1) {
        this.drawLine(ctx, this.line.StartPoint, this.line.EndPoint, img, this.lineWidth, this.lineColor)
        let {width, height} = img
         let start = this.direction.StartPoint
        let end = this.direction.EndPoint
        start = { x: start.x * width, y: start.y * height}
        end = { x: end.x * width, y: end.y * height}
        this.drawArrow(ctx, start.x, start.y, end.x, end.y, 30, 10, this.lineWidth, this.lineColor, false, true)
      } 
      if (this.drawType === 2) {
        this.drawPolyLine(ctx, this.polyLine, this.imgSize, this.lineWidth, this.lineColor)
        let {width, height} = this.imgSize
        let start = this.direction.StartPoint
        let end = this.direction.EndPoint
        start = { x: start.x * width, y: start.y * height}
        end = { x: end.x * width, y: end.y * height}
        this.drawArrow(ctx, start.x, start.y, end.x, end.y, 30, 10, this.lineWidth, this.lineColor, false, true)
      }
      // 注意这里的图片一定是同域的， 不然就要求图片服务器允许跨域访问，不然会因为安全问题报错
      return canvas.toDataURL("image/jpeg", 1)
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
    // 计算图片渲染宽高
    calcRenderSize(outWidth, outHeight, actualWidth, actualHeight) {
      let imgSize = {}
      if (actualWidth <= outWidth && actualHeight <= outHeight) { // 图片实际宽高都小于等于布局宽高
        imgSize = { width: actualWidth, height: actualHeight }
      } else if (actualWidth > outWidth || actualHeight > outHeight) { // 图片实际宽高都大于布局宽高
        if (outWidth / outHeight >= actualWidth / actualHeight) { // 如果布局斜率大于等于实际图片斜率, 则以布局高为图片高, 图片宽同比缩小
          imgSize = {width: outHeight / actualHeight * actualWidth  , height: outHeight}
        } else { // 如果布局斜率小于实际图片大小, 则以布局宽为图片宽, 图片高同比缩小
          imgSize = {width: outWidth  , height: outWidth / actualWidth * actualHeight}
        }
      } else if (actualWidth > outWidth) {  // 图片实际宽大于布局宽
        imgSize = {width: outWidth  , height: outWidth / actualWidth * actualHeight}
      } else { // 图片实际高大于布局高
        imgSize = {width: outHeight / actualHeight * actualWidth  , height: outHeight}
      }
      return imgSize
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