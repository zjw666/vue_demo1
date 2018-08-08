<template>
  <div id="app">
    <h1 class="title">新闻直播</h1>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
h1.title{
  text-align: center;
  font-size: 30px;
  margin: 20px auto;
}
#app{
  width: 100%;
}
</style>
