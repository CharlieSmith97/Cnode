<template>
<div class="pagination">
  <button @click="changeBtn">首页</button>
  <button @click="changeBtn">上一页</button>
  <button v-if="jduge" class="pageBtn">....</button>
  <button v-for="btn in pageBtns" :key="btn"
  :class="[{currentPage:btn === currentPage},'pageBtn']" @click="changeBtn(btn)">
  {{btn}}
  </button>

  <button @click="changeBtn">下一页</button>
</div>
</template>
<script>

import $ from 'jquery'
export default {
  name:'Pagination',
  data:function(){
    return {
      pageBtns:[1,2,3,4,5,'.....'],
      currentPage:1,
      jduge:false,
    }
  },methods:{
    changeBtn:function(page){
       //如果调用的该方法 如果说没有参数的话或者说参数类型不为 number类型! 则直接可以判定为 首页 上一页 下一页!
        if (typeof page !== 'number') {
          switch (page.target.innerText) {
            case '上一页':
            //将当前的页面往前点击
              $('button.currentPage').prev().click();
              break;
            case '下一页':
              $('button.currentPage').next().click();
              break;
            case '首页':
              // 按钮数组重置!
              this.pageBtns=[1,2,3,4,5,'.....'],
              this.changeBtn(1);
              break;
            default:
              break;
          }
          return ;
        }

        // 接下来便是page为页码的函数处理阶段!
        this.currentPage=page;
        if (page > 4) {
          // 上一页后面的按钮出现 ....
          this.jduge=true;
        }else{
          this.jduge=false;
        }
        // 如果点击的按钮 为第五个数的时候 按钮列表开始变化 前面的数据移除 后面出现第pageBtns[4]+1
        if (page === this.pageBtns[4]) {
          // 删除数组的第一个元素! 数组做表只剩下了3个
          this.pageBtns.shift();
          // 从索引位置删除元素! 对数组下标为4的地方插入 5+1=6
          this.pageBtns.splice(4,0,this.pageBtns[3]+1);
        }else if (page === this.pageBtns[0] && page !==1) {
            //先在第一个位置加一个
            this.pageBtns.unshift(this.pageBtns[0]-1);
            //移除最后一个数字
            this.pageBtns.splice(5,1);
        }

        // 发布事件! 到父组件!
        this.$emit('handleList',this.currentPage);
    }
  }
}
</script>
<style scoped>
 .pagination {
    margin-top: 12px;
    margin-bottom: 2px;
    background-color: white;
    padding: 6px 20px;
    border-radius: 5px;
    border: 1px solid #888888;
  }

  button {
    background-color: #fff;
    border: 1px solid #ddd;
    color: #778087;
    border-radius: 3px;
    outline: none;
    height: 21px;
    cursor: pointer;
    padding: 0 2px;
    width: 55px;
    height: 29px;
  }

  .pageBtn {
    position: relative;
    bottom: 1px;
    width: 40px;
    margin: 0 4px;
  }

  .currentPage {
    color: white;
    background-color: #1f1b1b;

  }
</style>
