<template>
  <div class="header">
    <div class="container">
      <router-link :to="{name:'root'}">
        <img src="../assets/cnodejs_light.svg">
      </router-link>
      <ul>
        <li>
          <router-link :to="{name:'root'}">首页</router-link>
        </li>
        <li><a href="#">新手入门</a></li>
        <li><a href="#">API</a></li>
        <li><a href="#">关于</a></li>
        <li><a href="#">注册</a></li>
        <li><a href="#">登录</a></li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Header"
  }
</script>

<style scoped>
  .header {
    background-color: #444;
    height: 50px;
  }

  img {
    max-width: 120px;
    margin-left: 50px;
    margin-top: 8px;
  }

  ul {
    list-style: none;
    float: right;
    margin: 4px 50px 4px 4px;
  }

  li {
    display: inline-block;
    padding: 10px 15px;
  }

  a {
    text-decoration: none;
    color: #ccc;
    font-size: 14px;
    text-shadow: none;
  }
</style>
