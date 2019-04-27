<template lang="pug">
    .PostList
      .loading(v-if='isLoading')
        span.double-bounce1
        span.double-bounce2
      div(v-if='DataIsLoaded')
        ul
          li
            .toobar
              span 全部
              span 精华
              span 分享
              span 文档
              span 招聘
          li(v-for='post in posts')
            img(:src='post.author.avatar_url', alt='')
            // 头像
            span.reply_count
              | {{post.reply_count}}/{{post.visit_count}}
            // 回复/浏览
            span(:class="[{put_good: (post.good === true),\
            put_top: (post.top === true),\
            'topiclist_tab': (post.good !== true && post.top !== true)}]")
              span
                | {{post | tabFormatter}}
              // 文章标签
              // <文章分类>
            router-link(:to="{\
            name:'post_content',\
            params:{\
            id:post.id,\
            name: post.author.loginname\
            }\
            }")
              span
                | {{post.title}}
            // 标题
            span.last_reply
              | {{post.last_reply_at | formaDate }}
            // 最后回复时间
          li
            pagination(@handleList='renderList')
</template>

<script>
import pagination from './Pagination'
  
  export default {
    name: 'PostList',
    data(){
      return {
        isLoading: false,
        DataIsLoaded: false,
        posts: [], //拿到页面数据
        postpage:1
      }
    },
    components:{
      pagination
    },
    methods:{
      getData(){
        this.isLoading = true,
        this.$http.get('https://cnodejs.org/api/v1/topics',{
          params: {
            page: this.postpage,
            limit: 20
          }
        })
          .then(res => {
            this.isLoading = false
            this.DataIsLoaded = true
            console.log(res)
            this.posts = res.data.data
          })
          .catch(err => {
            console.log(err)
          })
      },
      renderList(value){
        this.postpage = value
        this.getData()
      }
    },
    beforeMount(){
    this.getData()
    }
  }
</script>

<style lang='scss' scoped>
  @import '../assets/loading';
  @mixin border-radius($radius) {
            border-radius: $radius;
        -ms-border-radius: $radius;
      -moz-border-radius: $radius;
    -webkit-border-radius: $radius;
  }
  
  .PostList {
    .toobar {
      height: 40px;
      background-color: #f5f5f5;
      span {
        &:hover {color: #9e78c0;}
        font-size: 14px;
        color: #80bd01;
        line-height: 40px;
        margin: 0 10px;
        cursor: pointer;   
       }
    }
  
    ul {
      list-style: none;
      width: 100%;
      max-width: 1344px;
      margin: 0 auto;
      border:2px solid #e1e1e1;
      li:not(:first-child) {
        &:hover {background: #f5f5f5;}
        padding: 9px;
        font: {
          size: 15px;
          family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
          weight: 400px;
        }
        background-color: white;
        color: #333;
        border-top: 1px solid #f0f0f0;
      }

      li {
        span{ 
          line-height: 30px;
        }
        img {
          height: 30px;
          width: 30px;
          vertical-align: middle;
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
          @include border-radius(3px);
          color: #fff;
          font-size: 12px;
          margin-right: 10px;
        }
        .topiclist_tab {
          background-color: #e5e5e5;
          color: #999;
          padding: 2px 4px;
          @include border-radius(3px);
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
        a {
          &:hover{text-decoration: underline;}
          text-decoration: none;
          color: black;
        }
      }
    }    
  }
</style>