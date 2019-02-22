<template>
  <div id="app">
    <Header></Header>
   <div class="main">
     <!-- 路由视图 通过name的value进行映射! -->
     <router-view name="slideBar"></router-view>
     <router-view name="main"></router-view>
   </div>
  </div>
</template>

<script>
import Header from './components/Header'
import PostList from './components/PostList'
export default {
  name: 'App',
  components:{
    Header,PostList
  }
}
</script>

<style>
.main{
  width: 80%;
  margin: 0 auto;
}
</style>
