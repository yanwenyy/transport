<template>
    <div class="img-model" >
      <div class="img-btn">
        <i @click="transformImg" class="el-icon-refresh-right">旋转</i>
        <i @click="big" class="el-icon-zoom-in">放大</i>
        <i @click="small" class="el-icon-zoom-out">缩小</i>
        <i @click="close" class="el-icon-circle-close">关闭</i>
      </div>
      <img ref="bigImage" @click="close" :src="src" alt="">
      <!--<img @mousewheel="rollImg(this)" ref="bigImage" @click="close" :src="src" alt="">-->
    </div>
</template>

<script>
    export default {
        name: "img-pre",
        data(){
          return{
            src:'',
            current:0,
            zoom:1
          }
        },
        methods:{
          init (src) {
            this.src=src;
          },
          close(){
            this.$emit('refreshClose');
            this.$refs.bigImage.style.maxWidth="70%";
          },
          rollImg() {
            /* 获取当前页面的缩放比
                  若未设置zoom缩放比，则为默认100%，即1，原图大小
              */
            this.$refs.bigImage.style.maxWidth="100%";
            var delta = (event.wheelDelta && (event.wheelDelta > 0 ? 1 : -1)) ||  // chrome & ie
              (event.detail && (event.detail > 0 ? -1 : 1));
            var zoom = parseInt(this.$refs.bigImage.style.zoom) || 100;
            if (delta > 0) {
              // 向上滚
              zoom += event.wheelDelta / 12;
              this.$refs.bigImage.style.zoom = zoom + "%";

            } else if (delta < 0) {
              // 向下滚
              zoom += event.wheelDelta / 12;
              this.$refs.bigImage.style.zoom = zoom + "%";
            }
            // if (zoom >= 100 && zoom <250) {
            //   this.$refs.bigImage.style.zoom = zoom + "%";
            // }
            return false;
          },
          transformImg(){
            this.current = (this.current+90)%360;
            this.$refs.bigImage.style.transform = 'rotate('+this.current+'deg)';
          },
          big(){
            this.$refs.bigImage.style.maxWidth="100%";
            // var zoom = parseInt(this.$refs.bigImage.style.zoom) || 100;
            // console.log(zoom)
            this.zoom += 0.1;
            this.$refs.bigImage.style.transform = "scale("+this.zoom,+","+this.zoom+")"
          },
          small(){
            // this.$refs.bigImage.style.maxWidth="100%";
            // var zoom = parseInt(this.$refs.bigImage.style.zoom) || 100;
            // console.log(zoom)
            this.zoom -= 0.1;
            this.$refs.bigImage.style.transform = "scale("+this.zoom,+","+this.zoom+")"
          }
        }
    }
</script>

<style scoped>
  .img-model{
    width: 100%;
    height: 100vh;
    background: rgba(0,0,0,.3);
    position: fixed;
    top:0;
    left:0;
    z-index: 9999;
    overflow: auto;
    text-align: center;
    padding:40px 0;
    align-items: center;
  }
  .img-model>img{
    max-width: 70%;
    width: auto;
    height: auto;
  }
  .img-btn{
    width: 100%;
    height: 60px;
    position: absolute;
    top:10px;
    left:0;
    color:#fff;
    margin-bottom: 10px;
    z-index: 999;
    text-align: center;
  }
  .img-btn>i{
    display: inline-block;
    font-size: 24px;
    margin-right: 20px;
    cursor: pointer;
  }
</style>
