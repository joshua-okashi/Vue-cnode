<template lang="pug">
  .article
    div
      // 文章内容
      .topic_header
        .topic_title {{post.title}}
        ul
          li 发布于：{{post.create_at | formaDate}}
          li 作者: {{post.author.loginname}}
          li {{post.visit_count}}次浏览
          li 来自 {{post | tabFormatter}}
        .topic_content(v-html='post.content')
      // 用户回复
      #reply
        .topbar 回复
        .replySec(v-for='(reply,index) in post.replies')
          .replyUp
            router-link(:to="{\
            name:'user_info',\
            params: {\
            name:reply.author.loginname\
            }\
            }")
              img(:src='reply.author.avatar_url', alt='')
            router-link
              span {{reply.author.loginname}}
            span {{index+1}}楼
            span(v-if='reply.ups.length > 0')
              | 赞：{{reply.ups.length}}
            p(v-html='reply.content')
</template>

<script>
  export default {
    name:'Article',
    data() {
      return {
        post: {}
      }
    },
    methods: {
      getArticleData(){
        this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then(res => {
          if(res.data.success == true) {
            console.log(res)
            this.post = res.data.data
          }
        }) 
        .catch( err => {
          console.log(err)
        })
      }
    },
    beforeMount(){
      this.getArticleData()
    },
    watch: {
      '$route'(to,from){
        this.getArticleData()
        // 监视路由变化。
      }
    }
  }
</script>

<style lang='scss'>
  @import url("../assets/github-markdown");

  .article {
    &:not(:first-child){
      margin-right: 340px;
      margin-top: 15px;
    }
    // 文章主题
    .topic_header {
      padding: 10px;
      background-color: #fff;
      .topic_title {
        font-size: 20px;
        font-weight: bold;
        padding-top: 8px;
      }
      ul {
        list-style: none;
        padding: 0px 0px;
        margin: 6px 0px;
      }
      li {
        display: inline-block;
        font-size: 12px;
        color: #838383;
      }
      .topic_content {
        border-top: 1px solid #e5e5e5;
        padding: 0 10px;
      }
    }
    // 用户评论
    #reply {
      background-color: #fff;
      margin-top: 15px;
      .topbar {
        padding: 10px;
        background-color: #f6f6f6;
        height: 16px;
        font-size: 12px;
        margin-top: 10px;
      }
      img {
        width: 30px;
        height: 30px;
        position: relative;
        bottom: -9px;
      }
      .replyUp a:nth-of-type(2) {
        margin-left: 0px;
        display: inline-block;
      }
      .replySec{
        border-bottom:1px solid #e5e5e5;
        padding:0 10px;
      }
      a,span {
        font-size: 14px;
        color: #666;
        text-decoration: none;
      }
      p {
        padding: 4px 0;
      }
    }
  }

  .loading {
    text-align: center;
    padding-top: 300px;
  }

  .markdown-text img {
    width: 92% !important;
  }
</style>