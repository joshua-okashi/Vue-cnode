<template lang="pug">
  .userInfomation
    section
      img(:src='userinfo.avatar_url', alt='')
      span {{userinfo.loginname}}
      p {{userinfo.score}}积分
      p 注册时间：{{userinfo.create_at | formaDate}}
    .replies
      p 回复的主题
      ul
        li(v-for='item in userinfo.recent_replies')
          router-link(:to="{\
          name: 'post_content',\
          params:{\
          id: item.id\
          }\
          }\
          ")
            | {{item.title}}
    .topics
      p 创建的主题
      ul
        li(v-for='item in userinfo.recent_replies')
          router-link(:to="{\
          name: 'post_content',\
          params:{\
          id: item.id\
          }\
          }\
          ")
            | {{item.title}}

</template>

<script>
  export default {
    name:'UserInfo',
    data(){
      return{
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
    beforeMount() {
      this.getData();
    }
  }
</script>

<style lang='scss' scoped>
  .userInfomation {
    background: rgb(255, 255, 255);
    width: 75%;
    margin: 10px auto;
    section {
      padding: 12px;
    }
    img {
      width: 30px;
    }
    li {
      list-style:none;
    }
    .replies,
    .topics {
      font-size: 0.72rem;
      border-top: 10px #DDDDDD solid;
    }
    > div > p {
      padding: 12px 0 12px 12px;
      background-color: rgba(212, 205, 205, 0.17);
      font-size: 0.75rem;
      margin: 0;
    }
    > div >ul > li {
      padding: 4px 0 4px 12px;
      font-size: 0.72rem;
      text-overflow: ellipsis;
      overflow: hidden;
      white-space: nowrap;
      line-height: 30px;
    }
    > div >ul > li > a {
      color: #094E99;
      text-decoration: none;
    }
  }

</style>