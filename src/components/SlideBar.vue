<template>
  <div class="authorinfo">
    <div class="authorsummary">
      <div class="topbar">作者</div>
      <div class="inner">
        <router-link :to="{name:'user_info', params:{name: userinfo.loginname}}">
          <img :src="userinfo.avatar_url">
        </router-link>
        <span class="loginname">
          <router-link :to="{name:'user_info', params:{name: userinfo.loginname}}">
            {{userinfo.loginname}}
          </router-link>
        </span>
        <div>积分: {{userinfo.score}}</div>
      </div>
    </div>
    <div class="recent_topics">
      <div class="topbar">作者其它话题</div>
      <div class="inner">
        <ul>
          <li v-for="list in topiclimitby5">
            <router-link :to="{name:'post_content',params:{id: list.id,name:list.author.loginname}}">
              {{list.title}}
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "SlideBar",
    data() {
      return {
        userinfo: {}
      }
    },
    methods: {
      getAuthorData() {
        this.$http.get(`http://mock.hunger-valley.com/cnode/api/v1/user/${this.$route.params.name}`)
          .then(res => {
            this.isloading = false
            this.userinfo = res.data.data
          })
          .catch(err => {
            console.log(err)
          })
      }
    },
    computed: {
      topiclimitby5() {
        return this.userinfo.recent_topics ? this.userinfo.recent_topics.slice(0, 5) : ' '
      }
    },
    beforeMount() {
      this.getAuthorData()
    },
    watch: {
      '$route'(to, from) {
        this.getArticleData()
      }
    }
  }
</script>

<style scoped>
  .authorsummary, .recent_topics {
    background-color: #fff;
    border-radius: 3px;
  }

  .authorinfo {
    width: 328px;
    float: right;
    margin-top: 0;
  }

  li {
    padding: 3px 0;
  }

  .recent_topics ul {
    margin-top: 0px;
    margin-bottom: 0px;
    list-style: none;
  }

  ul a {
    font-size: 14px;
    text-decoration: none;
    color: #778087;
    text-overflow: ellipsis;
    overflow: hidden;
    line-height: 30px;
    max-width: 95%;
    white-space: nowrap;
    display: inline-block;
    vertical-align: middle;
  }

  .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    font-size: 13px;
    margin-top: 14px;
    color: #51585c;
    border-radius: 3px;
  }

  img {
    height: 48px;
    width: 48px;
    border-radius: 3px;
  }

  .loginname {
    width: 100px;
    float: right;
    margin-top: 10px;
    margin-right: 150px;
    font-size: 16px;
    vertical-align: middle;
    max-width: 120px;
    white-space: nowrap;
  }

  .loginname a {
    text-decoration: none;
    color: #778087;
  }

  .authorsummary .topbar {
    margin-top: 0px;
  }

  .inner {
    padding: 10px;
    line-height: 2em;
  }

  .authorsummary .inner div {
    color: #333;
    font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
    font-size: 15px;
  }
</style>
