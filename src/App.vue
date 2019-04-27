<template lang="pug">
    #app
      Header
      .main
        router-view(name='slidebar')
        router-view(name='main')
</template>

<script>
  import Header from './components/Header'
  import PostList from './components/PostList'

  export default {
  name: 'App',
  components:{
    Header,
    PostList
  }
}
</script>

<style lang='scss'>
* {
  margin: 0;
  padding: 0;
}
.main {
  width: 80%;
  margin: 0 auto;
}
</style>
