<template>
  <div class="autherinfo">

    <div class="loading" v-if="isLoading">
       <img src="../assets/loading.gif">
    </div>

    <div class="authersummay">
       <div class="topbar">作者</div>
       <!-- 点击图片跳转 -->
       <router-link :to="{
         name:'user_info',
         params:{
           name:userInfo.loginname
         }
       }">
        <img :src="userInfo.avatar_url" :alt="userInfo.loginname" >
       </router-link>


       <p>积分:{{userInfo.score}}</p>
    </div>

    <div class="recent_topics">
      <div class="topbar">作者最近主题</div>
      <!-- 通过点击标题然后通过 router-link 路由跳转到对应的 Article(文章) -->
      <ul>
        <li v-for="list in topicLimitBy5" :key="list.id">
          <router-link :to="{
            name:'post_content',
            params:{
              id:list.id,
              name:list.author.loginname,
            }
          }">{{list.title}}</router-link>
        </li>
      </ul>
    </div>

    <div class="recent_replies">
      <div class="topbar">作者最近回复</div>
      <ul>
        <li v-for="list in repliesLimitBy5" :key="list.id">
            <router-link :to="{
            name:'post_content',
            params:{
              id:list.id,
              name:list.author.loginname,
            }
          }">{{list.title}}</router-link>
        </li>
      </ul>
    </div>



  </div>
</template>
<script>
export default {
  name:'SlideBar',
  data:function(){
    return {
      isLoading:false,
      userInfo:{}
    }
  },methods:{
    getData:function(){
      this.$axios.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
      .then((response)=>{
        this.isLoading=false;
        this.userInfo=response.data.data;
      }).catch((error)=>{
        //当一步操作捕获到了错误的时候 打印出对应的错误信息!
        console.log(error);
      })
    }
  },computed:{
    topicLimitBy5:function(){
      /*
      如果用户信息中的最近的主题存在的话 那么就将其截取0~5 并且返回出来!
      否则的话 那么就直接返回其信息!
      */
      if (this.userInfo.recent_topics) {
        return  this.userInfo.recent_topics.slice(0,5);
      }else{
         this.userInfo.recent_topics;
      }
    },repliesLimitBy5:function(){
      if (this.userInfo.recent_replies) {
        return this.userInfo.recent_replies.slice(0,5);
      }else{
        return this.userInfo.recent_replies;
      }
    }
  },beforeMount(){
    this.isLoading=true;
    this.getData();
  }
}
</script>
<style scoped>
 .authersummay, .recent_replies, .recent_topics {
    background-color: #fff;
  }
  .autherinfo {
    width: 328px;
    float: right;
    margin-top: 0;
  }
  li{
    padding: 3px 0 ;
  }
  .recent_replies ul, .recent_topics ul {
    margin-top: 0px;
    margin-bottom: 0px;
    list-style: none;
    padding-left: 14px;
  }

  ul a {
    font-size: 12px;
    text-decoration: none;
    color: #778087;
  }

  .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    height: 16px;
    font-size: 12px;
    margin-top: 10px;
  }

  img {
    height: 48px;
    width: 48px;
    border-radius: 3px;
    margin-top: 10px;
    margin-left: 10px;
  }

  .loginname {
    width: 100px;
    float: right;
    margin-top: 22px;
    margin-right: 159px;
    font-size: 14px;
  }

  .loginname a {
    text-decoration: none;
    color: #778087;
  }

  .authersummay .topbar {
    margin-top: 0px;
  }
  a{
    text-decoration: none;

  }
</style>
