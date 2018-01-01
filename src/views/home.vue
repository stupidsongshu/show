<template>
  <div class="home">
    <!-- Wrap the image or canvas element with a block element (container) -->
    <div>
      <img id="image" src="../assets/img/picture.jpg">
    </div>

    <div style="border:2px dashed red;">
      <p>
        图片上传前预览：<input type="file" id="xdaTanFileImg" @change="xmTanUploadImg(this)" accept="image/*"/>
        <input type="button" value="隐藏图片" onclick="document.getElementById('xmTanImg').style.display = 'none';"/>
        <input type="button" value="显示图片" onclick="document.getElementById('xmTanImg').style.display = 'block';"/>
      </p>
      <img id="xmTanImg"/>
      <div id="xmTanDiv"></div>
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
      xmTanUploadImg(obj) {
        console.log(obj);

        var file = obj.files[0];

        console.log(file);
        console.log("file.size = " + file.size);  //file.size 单位为byte

        var reader = new FileReader();

        //读取文件过程方法
        reader.onloadstart = function (e) {
          console.log("开始读取....");
        }
        reader.onprogress = function (e) {
          console.log("正在读取中....");
        }
        reader.onabort = function (e) {
          console.log("中断读取....");
        }
        reader.onerror = function (e) {
          console.log("读取异常....");
        }
        reader.onload = function (e) {
          console.log("成功读取....");

          var img = document.getElementById("xmTanImg");
          img.src = e.target.result;
          //或者 img.src = this.result;  //e.target == this
        }

        reader.readAsDataURL(file)
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
