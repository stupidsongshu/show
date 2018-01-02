<template>
  <div class="home">
    <!-- Wrap the image or canvas element with a block element (container) -->
    <div>
      <img id="image" src="../assets/img/picture.jpg">
    </div>

    <div style="border:2px dashed red;">
      <p>
        选择图片：<input type="file" id="xdaTanFileImg" @change="xmTanUploadImg($event)" accept="image/*" />
        <input type="button" value="隐藏图片" onclick="document.getElementById('imgTarget').style.display = 'none';"/>
        <input type="button" value="显示图片" onclick="document.getElementById('imgTarget').style.display = 'block';"/>
        <input type="button" value="删除图片" onclick="document.getElementById('imgTarget').style.display = 'block';"/>
        <input type="button" value="编辑图片" @click="editorImg"/>
      </p>
      <img id="imgTarget"/>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Cropper from 'cropperjs'
  export default {
    mounted() {
      //判断浏览器是否支持FileReader接口
      if (typeof FileReader === 'undefined') {
        document.getElementById("xmTanDiv").InnerHTML = "<h1>当前浏览器不支持FileReader接口</h1>";
        //使选择控件不可操作
        document.getElementById("xdaTanFileImg").setAttribute("disabled", "disabled");
      }

      var image = document.getElementById('image');
      var cropper = new Cropper(image, {
        // aspectRatio: 16 / 9,
        crop: function(e) {
          console.log(e.detail.x);
          console.log(e.detail.y);
          console.log(e.detail.width);
          console.log(e.detail.height);
          console.log(e.detail.rotate);
          console.log(e.detail.scaleX);
          console.log(e.detail.scaleY);
        }
      });
    },
    methods: {
      //选择图片，马上预览
      xmTanUploadImg(e) {
        /**
         * 从 file 域获取 本地图片 url
         */
        // function getFileUrl(obj) {
        //   let url;
        //   url = window.URL.createObjectURL(obj.files.item(0));
        //   return url;
        // }
        console.log(e)
        let that = this
        let file = e.target.files[0]
        let imageSize =file.size / 1024
        if (imageSize > 100) {
          alert('请上传100k以下图片')
        } else {
          let reader = new FileReader()
          reader.readAsDataURL(file)
          reader.onload = function() {
            var dataUrl = reader.result
            document.getElementById('imgTarget').src = dataUrl
          }
        }
      },
      editorImg() {
        let imgTarget = document.getElementById('imgTarget')
        console.log(imgTarget)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .home
    width: 100%
    height:100%
  /* Limit image width to avoid overflow the container */
  img
    max-width: 100% /* This rule is very important, please do not ignore this! */
</style>
