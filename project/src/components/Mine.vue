<template>
  <div class="mine">
    <h1>我的</h1>
    <img src = "../assets/logo.png" class="user"/>
    <span class="name">用户昵称</span>
    <ul class="list">
      <li>
        <svg class="icon pic" aria-hidden="true">
          <use xlink:href="#icon-icon-xiaoxi"></use>
        </svg>
        <span class="word">消息</span>
        <svg class="icon btn" aria-hidden="true">
          <use xlink:href="#icon-xiangqing"></use>
        </svg>
      </li>
      <li>
        <svg class="icon pic" aria-hidden="true">
          <use xlink:href="#icon-chakantieziguanzhu"></use>
        </svg>
        <span class="word">我的偶像</span>
        <svg class="icon btn" aria-hidden="true">
          <use xlink:href="#icon-xiangqing"></use>
        </svg>
      </li>
      <li>
        <svg class="icon pic" aria-hidden="true">
          <use xlink:href="#icon-msnui-bind"></use>
        </svg>
        <span class="word">账号绑定</span>
        <svg class="icon btn" aria-hidden="true">
          <use xlink:href="#icon-xiangqing"></use>
        </svg>
      </li>
      <li>
        <svg class="icon pic" aria-hidden="true">
          <use xlink:href="#icon-kejian"></use>
        </svg>
        <span class="word">设置</span>
        <svg class="icon btn" aria-hidden="true">
          <use xlink:href="#icon-xiangqing"></use>
        </svg>
      </li>
    </ul>
  </div>
</template>

<script>
import zjw from '../assets/iconfont.js';

export default {
  name: 'Mine',
  mounted() {
    zjw();
  },
};
</script>

<style scoped>
.icon {
   width: 1.5em; height: 1.5em;
   vertical-align: -0.15em;
   fill: currentColor;
   overflow: hidden;
}
h1{
    text-align: center;
}
.name{
    font-size: 12px;
    position: relative;
    top:-20px;
    left: 10px;;
}
.list{
    list-style-type:none;
    font-size: 12px;
    padding: 0;
}
.user{
    border-radius: 50%;
    border: 1px solid #ddd;
    width: 50px;
    margin-left: 10px;
}
.mine{
    margin: 0 auto;
    width: 400px;
    border: 1px solid #ddd;
}
.btn{
    float: right;
    padding-top: 5px;
}
li{
    margin: 10px auto;
    height: 50px;
    width: 350px;
}
.pic{
    width: 2.5em; height: 2.5em;
}
.word{
    position: relative;
    top: -8px;
    left: 5px;
}
</style>
