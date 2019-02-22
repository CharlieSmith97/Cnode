<template>
  <div class="PostList">
    <!-- 调用数据未返回的时候 开启加载的GIF动画! -->
    <div class="loading"  v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <div class="posts" v-else>
      <ul>
        <!-- 栏目分类 -->
        <li>
          <div class="toobar">
              <span @click="change" :class="[tab === '' ? 'active':'deactive']">全部</span>
            <span @click="change" :class="[tab === 'good' ? 'active':'deactive']">精华</span>
            <span @click="change" :class="[tab === 'share' ? 'active':'deactive']">分享</span>
            <span @click="change" :class="[tab === 'ask' ? 'active':'deactive']">问答</span>
            <span @click="change" :class="[tab === 'job' ? 'active':'deactive']">招聘</span>
          </div>
        </li>

        <li v-for="post in posts" :key='post.id'>
          <!-- 每一个行中的信息 -->
            <img :src="post.author.avatar_url" alt="">
            <!-- 帖子的恢复和浏览 -->
            <span class="allcount">
            <span class="reply_count">{{post.reply_count}}</span>
            /{{post.visit_count}}
            </span>
            <!-- 帖子的分类 -->
            <span :class="[{put_good:(post.good === true),put_top:(post.top === true),'topiclist-tab':(post.good !== true && post.top !==true)}]">
              <span>
                <!-- 表单格式化 -->
                {{post|tabFormatter}}
              </span>
            </span>
            <!-- title帖子标题 -->
            <!-- 路由链接 -->
            <router-link :to="{
              name:'post_content',
              params:{
                id:post.id,
                name:post.author.loginname
                }
              }">
              <span>{{post.title}}</span>
            </router-link>
            <span class="last_reply">
              <!-- 使用{{}}插值 并且可以通过 竖线后面跟 过滤器filter -->
              {{post.last_reply_at | formatDate}}
            </span>
        </li>

        <li>
            <!-- 自定义事件! 调用的方法为renderList -->
        <Pagination @handleList="renderList"></Pagination>
        </li>

      </ul>
    </div>
  </div>
</template>
<script>
import Pagination from './Pagination'
export default {
  name:'PostList',
  data:function(){
    return {
      isLoading:false,
      posts:[],
      // postpage post页! 默认是首页!
      postpage:1,
      tab:''
    }
  },components:{
    Pagination
  },methods:{
    getData:function(){
      this.$axios.get('https://cnodejs.org/api/v1/topics',{
        params:{
          // 页面为当前的页面值!
          page:this.postpage,
          limit:20,
          tab:this.tab,
        }
      })
      .then((response)=>{
         if (response.data.success) {
        this.isLoading=false;
        this.posts=response.data.data;
        }
      }).catch((error)=>{
        //当一步操作捕获到了错误的时候 打印出对应的错误信息!
        console.log(error);
      })
      // 渲染列表
    },renderList:function(value){
      this.postpage=value;
      this.getData();
    },change:function(event){
      switch (event.currentTarget.innerText) {
        case '全部':
           this.tab = ''
          break;
        case '精华':
           this.tab = 'good'
          break;
        case '分享':
           this.tab = 'share'
          break;
        case '问答':
           this.tab = 'ask'
          break;
        case '招聘':
           this.tab = 'job'
          break;
        default:
           this.tab=''
          break;
      }
      this.getData();
    }
  },beforeMount:function(){
    this.isLoading=true;//当组件未挂载之前 启用加载动画!
    this.getData();//并且获取数据!
  }
}
</script>
<style scoped>
  .PostList{
    background-color: #e1e1e1;
  }
  .posts {
    margin-top: 10px;
  }

  .PostList img {
    height: 30px;
    width: 30px;
    vertical-align: middle;
  }

  ul {
    list-style: none;
    width: 100%;
    max-width: 1344px;
    margin: 0 auto;
  }

  ul li:not(:first-child) {
    padding: 9px;
    font-size: 15px;
    font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
    font-weight: 400;
    background-color: white;
    color: #333;
    border-top: 1px solid #f0f0f0;
  }

  li:not(:first-child):hover {
    background: #f5f5f5;;
  }

  li:last-child:hover {
    background: white;
  }

  li span {
    line-height: 30px;
  }

  .allcount {
    width: 70px;
    display: inline-block;
    text-align: center;
    font-size: 12px;
  }

  .reply_count {
    color: #9e78c0;
    font-size: 14px;
  }

  .put_good, .put_top {
    background: #80bd01;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    color: #fff;
    font-size: 12px;
    margin-right: 10px;
  }

  .topiclist-tab {
    background-color: #e5e5e5;
    color: #999;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    font-size: 12px;
    margin-right: 10px;
  }

  .last_reply {
    text-align: right;
    min-width: 50px;
    display: inline-block;
    white-space: nowrap;
    float: right;
    color: #778087;
    font-size: 12px;
  }

  .toobar {
    height: 40px;
    background-color: #f5f5f5;
  }

  .toobar span {
    font-size: 14px;
    color: #80bd01;
    line-height: 40px;
    margin: 0 10px;
    cursor: pointer;
  }

  .toobar span:hover {
    color: #9e78c0;
  }

  a {
    text-decoration: none;
    color: black;
  }

  a:hover {
    text-decoration: underline;
  }

  .loading {
    text-align: center;
      max-width: 100%;
      max-height: 100%;
  }
</style>
