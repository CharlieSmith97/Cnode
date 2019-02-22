<template>
  <div class="header">
    <router-link :to="{
      name:'root'
    }"><img src="../assets/cnodejs_light.svg" ></router-link>
    <ul>
      <li><a href="root">首页</a></li>
      <li><a href="#">新手入门</a></li>
      <li><a href="#">API</a></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">登录</a></li>
      <li><a href="#">注册</a></li>
    </ul>
  </div>
</template>
<script>
export default {
  name:'Header'
}
</script>
<style scoped>
.header{
background-color:#333333;
height: 50px;
margin-top: 0px;
}
img{
  max-width: 120px;
  margin-left: 50px;
  margin-top: 7px;
}

ul{
  list-style:none;
  float:right;
}
li{
  display: inline-block;
  padding:0px 5px;
}
 a{
  text-decoration: none;
  color:white;
}
</style>
