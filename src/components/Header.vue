<template lang="pug">
  .header
    router-link(:to="{name: 'root'}")
      img(src='../assets/cnode.svg', alt='')
    ul.clearfix
      li
        a(href='') 首页
      li
        a(href='') 新手入门
      li
        a(href='') API
      li
        a(href='') 关于
      li
        a(href='') 注册
      li
        a(href='') 登陆
</template>

<script>
  export default {
    name: 'Header'
  }
</script>

<style lang='scss' scoped>
  .clearfix::after {
    content: '';
    display: block;
    clear: both;
  }

  .header {
    background-color: #444444;
    height: 50px;
    img {
      max-width: 120px;
      margin: {
        left: 50px;
        top: 10px;
      }
    }
    ul {
      list-style: none;
      float: right;
      margin: 4px;
      li {
        display: inline-block;
        padding: 10px 15px;
      }
      a {
        color: #ccc;
        font-size: 14px;
        text: {
          shadow: none;
          decoration: none;
        }
      }
    }
  }





</style>