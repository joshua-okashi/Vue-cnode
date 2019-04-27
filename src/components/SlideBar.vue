<template lang="pug">
  .authorinfo
    // 作者头像
    .authorsummary
      .topbar 作者
      router-link(:to="{\
      name:'user_info',\
      params:{\
      name: userinfo.loginname\
      }\
      }")
        img(:src='userinfo.avatar_url', alt='')
    // 作者主题栏
    .recent_topics
      .topbar 作者最近主题
      ul
        li(v-for='list in topiclimitby5')
          router-link(:to="{\
          name: 'post_content',\
          params: {\
          id: list.id,\
          name: list.author.loginname\
          }\
          }")
            | {{list.title}}
    // 作者回复栏
    .recent_replies
      .topbar 作者最近回复
      ul
        li(v-for='list in replylimitby5')
          router-link(:to="{\
          name: 'post_content',\
          params: {\
          id: list.id,\
          name: list.author.loginname\
          }\
          }")
            | {{list.title}}
</template>

<script>
  export default {
    name:'SlideBar',
    data(){
      return {
        userinfo: {}
      }
    },
    methods: {
      getData(){
        this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
            console.log(res)
            this.userinfo = res.data.data
        }) 
        .catch( err => {
          console.log(err)
        })
      }
    },
    computed:{
      topiclimitby5(){
        if(this.userinfo.recent_topics){
          return this.userinfo.recent_topics.slice(0,5)
        }
      },
      replylimitby5(){
          if(this.userinfo.recent_replies){
          return this.userinfo.recent_replies.slice(0,5)
        }
      }
    },
    beforeMount() {
      this.getData()
    },
     watch: {
      '$route'(to,from){
        this.getData()
        // 监视路由变化。
      }
    }
  }
</script>

<style lang='scss' scoped>

  .authorinfo {
    width: 328px;
    float: right;
    margin-top: 0;
  
    .authorsummay,
    .recent_replies,
    .recent_topics {
      background-color: #fff;
    }
    .recent_replies ul, 
    .recent_topics ul {
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
    li {
      padding: 3px 0 ;
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
      margin: 10px;
    }
    .loginname {
      width: 100px;
      float: right;
      margin-top: 22px;
      margin-right: 159px;
      font-size: 14px;
      a {
        text-decoration: none;
        color: #778087;
      }
    }
  }
</style>