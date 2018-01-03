<template>
  <div class="home">
    <div style="border:2px dashed red;">
      <p>
        选择图片：<input type="file" id="xdaTanFileImg" @change="xmTanUploadImg($event)" accept="image/*" />
        <input type="button" value="隐藏图片" onclick="document.getElementById('imgTarget').style.display = 'none';"/>
        <input type="button" value="显示图片" onclick="document.getElementById('imgTarget').style.display = 'block';"/>
        <input type="button" value="删除图片" onclick="document.getElementById('imgTarget').style.display = 'block';"/>
        <input type="button" value="编辑图片" @click="editorImg"/>
        <input type="button" value="保存" @click="editorImgSave"/>
        <input type="button" value="重置" @click="editorImgReset"/>
      </p>
    </div>
    <img id="imgTarget"/>
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
          // 读出 base64
          reader.readAsDataURL(file)
          reader.onload = function() {
            // 图片的 base64 格式, 可以直接当成 img 的 src 属性值
            var dataUrl = reader.result
            document.getElementById('imgTarget').src = dataUrl
          }
        }
      },
      editorImg() {
        let imgTarget = document.getElementById('imgTarget')

        var cropper = new Cropper(imgTarget, {
          // aspectRatio: 16 / 9,
          crop: function(e) {
            console.log(e.detail.x);
            console.log(e.detail.y);
            console.log(e.detail.width);
            console.log(e.detail.height);
            console.log(e.detail.rotate);
            console.log(e.detail.scaleX);
            console.log(e.detail.scaleY);
          },
          dragMode: 'move',
          aspectRatio: 0,
          responsive: true,
          modal: false,
          guides: true,
          center: false,
          highlight: true
        });
      },
      editorImgSave() {},
      editorImgReset() {

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
