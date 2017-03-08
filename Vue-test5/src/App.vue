<template>
  <div id="app" :style="backgroundDiv" class="background-img">
    <router-view></router-view>
  </div>
</template>

<style>
 @import 'assets/css/normalize.min.css';
 @import 'assets/css/flex.css';

html,body{
  width: 100%;
  height: 100%;
  overflow : hidden; 
}


#app{
  height: 100%;
  /*background: url(assets/img/bg.jpg) no-repeat center; */
  background-size: cover;
}


</style>

<script>


export default {
  name: 'app',
  data: function(){
        return{
          message : "",
          backgroundDiv: {
            backgroundImage: 'url(' + require('assets/img/bg.jpg') + ')',
          }
        } 
    },
    methods: {
       
    }
}




</script>


