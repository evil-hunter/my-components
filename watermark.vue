<template>
  <div ref="watermark" class="watermark"/>
</template>

<script>
  export default {
    mounted() {
      const water = this.$refs.watermark;
      console.log(water, 'water');
      this._setWaterMark(water, 400, 400, 'watermark');
    },
    methods: {
      /**
       * canvas转换成图片
       */
      _canvasWM (width, height, content, rotate = 30) {
        // 创建canvas
        let canvas = document.createElement('canvas');
        canvas.setAttribute('width', width);
        canvas.setAttribute('height', height);
        let ctx = canvas.getContext("2d");
        // 水印内容, 文字样式
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.font = "20px microsoft yahei";
        ctx.fillStyle = 'rgba(0, 0, 0, 1)';
        ctx.rotate(Math.PI / 180 * rotate);
        ctx.fillText(content, parseFloat(width) / 2, parseFloat(height) / 2);
        // canvas转换成base64
        let base64Url = canvas.toDataURL();
        console.log(base64Url, 'base64Url');
        return base64Url;
      },
      /**
       * 制作水印
       */
      _setWaterMark (container, width, height, content) {
        const waterImg = this._canvasWM(width, height, content);
        container.setAttribute('style', `
        background-image:url('${waterImg}')
      `);
      },
    },
  }
</script>

<style lang="css">
  .watermark {
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    opacity: 0.2;
    z-index: 9999999999;
    /* 用这个属性让上层遮罩不影响用户点击 */
    pointer-events: none
  }
</style>
