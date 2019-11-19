<template>
  <div class="article">
    <div class="loading" v-if="isloading">
      <img src="../assets/loading.gif">
    </div>
    <div v-else>
      <div class="topic_header">
        <div class="topic_title">{{post.title}}</div>
        <div class="changes">
          <span>发布于{{post.create_at | formatDate}}</span>
          <span>作者 {{post.author.loginname}}</span>
          <span>{{post.visit_count}} 次浏览</span>
          <span>来自 {{post | tabFormatter}}</span>
        </div>
        <div v-html="post.content" class="topic_content markdown-body"></div>
      </div>
      <div id="reply">
        <div class="topbar">{{post.reply_count}} 回复</div>
        <div class="replySec" v-for="(reply,index) in post.replies">
          <div class="replyUp">
            <router-link :to="{name:'user_info',params:{name: reply.author.loginname}}">
              <img :src="reply.author.avatar_url">
            </router-link>
            <router-link :to="{name:'user_info',params:{name: reply.author.loginname}}">
              <span class="reply_author">{{reply.author.loginname}}</span>
            </router-link>
            <span>{{index+1}}楼</span>
            <span>{{reply.create_at | formatDate}}</span>
            <div class="user_action">
              <span v-if="reply.ups.length>0">
                <i class="fa up_btn fa-thumbs-o-up" title="喜欢"></i>
                <span class="up_count">{{reply.ups.length}}</span>
              </span>
              <span v-else></span>
            </div>
          </div>
          <p v-html="reply.content" class="reply_content markdown-body"></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Article",
    data() {
      return {
        isloading: false,
        post: {}  //代表当前文章页的所有内容，所有属性
      }
    },
    methods: {
      getArticleData() {
        this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
          .then(res => {
            this.isloading = false
            this.post = res.data.success ? res.data.data : this.post
          })
          .catch(err => {
            console.log(err)
          })
      }
    },
    beforeMount() {
      this.isloading = true
      this.getArticleData()
    },
    watch: {
      '$route'(to, from) {
        this.getArticleData()
      }
    }
  }
</script>

<style>
  @import url('../../node_modules/github-markdown-css');

  #reply .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    font-size: 14px;
    margin-top: 10px;
  }

  .article {
    margin-top: 14px;
    margin-right: 10px;
  }

  .article:not(:first-child) {
    margin-right: 342px;
    margin-top: 14px;
  }

  #reply, .topic_header {
    background-color: #fff;
    border-radius: 3px;
  }

  #reply {
    margin-top: 15px;
  }

  #reply img {
    width: 30px;
    height: 30px;
    position: relative;
    bottom: -9px;
  }

  #reply a{
    font-size: 15px;
    text-decoration: none;
  }
  #reply span {
    font-size: 13px;
    color: #666;
    text-decoration: none;
  }

  .replySec {
    border-bottom: 1px solid #e5e5e5;
    padding: 0 10px;
  }

  .loading {
    text-align: center;
    padding-top: 300px;
  }

  .replyUp a:nth-of-type(2) {
    margin-left: 0px;
    display: inline-block;
  }

  .replyUp .reply_author {
    color: #666;
    font-size: 12px;
    font-weight: 700;
  }

  .replyUp .user_action {
    float: right;
    margin-left: 20px;
    font-size: 15px;
  }

  .replyUp .user_action .fa {
    font: normal normal normal 14px/1 FontAwesome;
    display: inline-block;
    font-size: inherit;
    text-rendering: auto;
    color: #000;
    opacity: .4;
  }

  .replyUp .user_action .up_btn {
    cursor: pointer;
    opacity: .4;
  }

  .replyUp .user_action .fa-thumbs-o-up::before {
    content: "\f087";
  }

  .up_count {
    color: gray;
  }

  .replySec .reply_content {
    padding-left: 40px;
    color: #333;
    margin: 2px 0 8px 0;
    font-size: 15px;
  }

  .topic_header {
    padding: 10px;
  }

  .topic_title {
    font-size: 22px;
    font-weight: 700;
    margin: 8px 0 0;
    display: inline-block;
    vertical-align: bottom;
    width: 75%;
    line-height: 130%;
    font-family: "Helvetica Neue","Luxi Sans","DejaVu Sans",Tahoma,"Hiragino Sans GB",STHeiti,sans-serif!important;
    color: #333;
  }

  .topic_header .changes {
    margin: 8px 0px;
    font-size: 12px;
    color: #838383;
  }

  .topic_header .changes span:before {
    content: "• ";
  }

  .topic_content {
    border-top: 1px solid #e5e5e5;
    padding: 8px 10px;
  }

</style>
