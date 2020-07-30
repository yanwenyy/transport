<template>
    <div class="img-model">
      <img @click="close" :src="src" alt="">
    </div>
</template>

<script>
    export default {
        name: "img-pre",
        data(){
          return{
            src:''
          }
        },
        methods:{
          init (src) {
            console.log(src);
            this.src=src;
          },
          close(){
            this.$emit('refreshClose')
          }
        }
    }
</script>

<style scoped>
  .img-model{
    width: 100vw;
    height: 100vh;
    background: rgba(0,0,0,.3);
    position: fixed;
    top:0;
    left:0;
    z-index: 9999;
    display: flex;
    justify-content: center;
    padding:20px 0;
    overflow: auto;
  }
  .img-model>img{
    width: 70%;
    height: auto;
  }
</style>
