<template>
  <div class="userInfo">
    <div class="loading" v-if="isloading">
      <img src="../assets/loading.gif">
    </div>
    <div class="userInformation" v-else>
      <section>
        <p class="user_header">
          <router-link :to="{name:'root'}">主页</router-link>
        </p>
        <div class="inner_userinfo">
          <div class="user_big_avatar">
            <img :src="userinfo.avatar_url">
          </div>
          <span>{{userinfo.loginname}}</span>
          <div>{{userinfo.score}} 积分</div>
          <p class="register_time">
            注册时间 {{userinfo.create_at | formatDate}}
          </p>
        </div>
      </section>
      <div class="topics">
        <p>最近创建的话题</p>
        <ul>
          <li v-for="item in userinfo.recent_topics">
            <!--头像-->
            <img :src="item.author.avatar_url">
            <!--标题-->
            <router-link :to="{name:'post_content',params:{id: item.id,name:item.author.loginname}}">
              {{item.title}}
            </router-link>
            <!--最终回复时间-->
            <span class="last_reply">
            {{item.last_reply_at | formatDate}}
            </span>
          </li>
        </ul>
      </div>
      <div class="replies">
        <p>最近参与的话题</p>
        <ul>
          <li v-for="item in userinfo.recent_replies">
            <!--头像-->
            <img :src="item.author.avatar_url">
            <!--标题-->
            <router-link :to="{name:'post_content',params:{id: item.id,name:item.author.loginname}}">
              {{item.title}}
            </router-link>
            <!--最终回复时间-->
            <span class="last_reply">
            {{item.last_reply_at | formatDate}}
            </span>
          </li>
        </ul>
      </div>
    </div>
  </div>

</template>

<script>
  export default {
    name: "UserInfo",
    data() {
      return {
        isloading: false,
        userinfo: {}
      }
    },
    methods: {
      getUserData() {
        this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
          .then(res => {
            this.isloading = false
            this.userinfo = res.data.data
          })
          .catch(err => {
            console.log(err)
          })
      }
    },
    beforeMount() {
      this.isloading = true
      this.getUserData()
    },
    watch: {
      '$route'(to, from) {
        this.getArticleData()
      }
    }
  }
</script>

<style scoped>
  .loading {
    text-align: center;
    padding-top: 300px;
  }

  .userInformation {
    background: white;
    width: 75%;
    margin: 14px auto;
  }

  .userInformation .inner_userinfo {
    padding: 10px;
    border-top: 1px solid #e5e5e5;
    border-radius: 0 0 3px 3px;
    background-color: #fff;
    line-height: 2em;
  }

  .userInformation .inner_userinfo .user_big_avatar {
    float: left;
    margin-right: 10px;
  }

  .userInformation .inner_userinfo span {
    color: #778087;
    font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
  }

  .userInformation .inner_userinfo .register_time {
    color: #ababab;
  }

  .userInformation section .user_header {
    padding: 12px 0 12px 12px;
    background-color: #f6f6f6;;
    color: #444;
    font-size: 14px;
  }

  .userInformation img {
    width: 30px;
  }

  .userInformation li {
    list-style: none;
  }

  .userInformation .replies,
  .userInformation .topics {
    font-size: 0.72rem;
    border-top: 12px #DDDDDD solid;
  }

  .userInformation > div > p {
    padding: 12px 0 12px 12px;
    background-color: #f6f6f6;;
    color: #444;
    font-size: 14px;
    margin: 0;
  }

  .userInformation > div img {
    border-radius: 3px;
    max-width: 100%;
    vertical-align: middle;
    border: 0;
  }

  .userInformation > div > ul > li {
    border-top: 1px solid #f0f0f0;
    padding: 10px 10px 8px 10px;
    white-space: nowrap;
    font-size: 0.72rem;
    text-overflow: ellipsis;
    overflow: hidden;
    line-height: 20px;
    vertical-align: middle;
  }

  .userInformation > div > ul > li > a {
    color: #08c;
    text-decoration: none;
    max-width: 80%;
    white-space: nowrap;
    display: inline-block;
    vertical-align: middle;
    font-size: 16px;
    line-height: 30px;
    margin-left: 10px;
    text-overflow: ellipsis;
    overflow: hidden;
  }

  .userInformation > div > ul > li > a:hover {
    color: #005580;
    text-decoration: underline;
  }

  .userInformation .last_reply {
    float: right;
    text-align: right;
    min-width: 50px;
    display: inline-block;
    white-space: nowrap;
    font-size: 11px;
    margin-left: 20px;
    color: #777;
  }

</style>
