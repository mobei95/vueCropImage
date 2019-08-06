<template>
  <div class="copper-box">
    <img class="copper-img" :src="src" ref="img">
  </div>
</template>
<script>
import Copper from 'cropperjs'
import 'cropperjs/dist/cropper.min.css'
export default {
  name: 'vue-crop',
  props: {
    src: {
      type: String,
      default: 'https://lh-static.playhudong.com/forever/image/funCoverDefault1.pngtapd_23436241_base64_1542093035_53%281%29.png'
    },
    aspectRatio: { // 裁剪框的比例
      type: Number,
      default: null
    },
    modal: {
      type: Boolean,
      default: true
    },
    cropBoxResizable: { // 是否允许拖动调整裁剪框的大小
      type: Boolean,
      default: true
    },
    getImg: {
      type: Function,
      default: function () {
        return null
      }
    }
  },
  data () {
    return {
      canvasImg: null
    }
  },
  mounted () {
    this.initCopper()
  },
  methods: {
    /**
     * 初始化
     * */
    initCopper () {
      let copper = new Copper(this.$refs.img, {
        viewMode: 1,
        // 定义裁剪器的拖动模式
        dragMode: 'none',
        // 裁剪框比例
        aspectRatio: this.aspectRatio,
        modal: this.modal,
        // 裁剪框单击两次，切换裁剪和拖动模式
        toggleDragModeOnDblclick: false,
        // 允许通过拖动调整裁剪框的大小
        cropBoxResizable: this.cropBoxResizable,
        checkCrossOrigin: false,
        // 定义自动裁剪区域的大小
        autoCropArea: 1,
        // 不允许旋转
        rotatable: false,
        ready: () => {
          this.callback()
        },
        cropend: () => {
          this.callback()
        }
      })
      this.copper = copper
    },

    /**
     * ready和cropend的callback
     * */
    callback () {
      this.cropEnd()
      /*if (this.getImg) {
        this.getImg(this.canvasImg)
      }*/
    },

    /**
     * 裁剪框移动结束
     * */
    cropEnd () {
      let data = this.copper.getData()
      this.createCanvas(data)
    },

    /**
     * 创建canvas
     * data: 裁剪区域的数据
     * */
    createCanvas (data) {
      let canvas = document.createElement('canvas')
      let ctx = canvas.getContext('2d')
      canvas.width = data.width
      canvas.height = data.height
      let img = new Image()
      img.setAttribute('crossOrigin', 'Anonymous')
      img.src = this.src
      img.onload = () => {
        // ctx.drawImage(img, 0, 0, data.width, data.height)
        this.canvasCover(canvas, ctx, data.width, data.height, data.x, data.y, img)
      }
    },

    /**
     * canvas覆盖
     * */
    canvasCover (canvas, ctx, width, height, x, y, img) {
      let dirtyCanvas = document.createElement('canvas')
      let dirtyContext = dirtyCanvas.getContext('2d')
      dirtyCanvas.width = img.width
      dirtyCanvas.height = img.height
      dirtyContext.drawImage(img, 0, 0, img.width, img.height)
      let imagedata = dirtyContext.getImageData(x, y, img.width, img.height)
      ctx.putImageData(imagedata, 0, 0, 0, 0, width, height);
      this.canvasImg = canvas.toDataURL('image/png', 1)
      this.$emit('copperImg', this.canvasImg)
    }
  }
}
</script>
<style scoped lang="scss">
  .copper {
    width: 100%;
    height: 100%;
    position: relative;
  }
  .canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .copper-box {
    width: 100%;
    height: 100%;
  }
  .copper-img {
    width: 100%;
    height: auto;
  }
</style>
