<template>
  <div class="PostList">
    <div class="topbar">
      <span>
        <router-link :to="{path:'/',query:{tab:'all'}}" class="topic-tab active" exact>全部</router-link>
      </span>
      <span>
        <router-link :to="{path:'/',query:{tab:'good'}}" class="topic-tab" exact>精华</router-link>
      </span>
      <span>
        <router-link :to="{path:'/',query:{tab:'share'}}" class="topic-tab" exact>分享</router-link>
      </span>
      <span>
        <router-link :to="{path:'/',query:{tab:'ask'}}" class="topic-tab" exact>问答</router-link>
      </span>
      <span>
        <router-link :to="{path:'/',query:{tab:'job'}}" class="topic-tab" exact>招聘</router-link>
      </span>
    </div>
    <div class="loading" v-if="isloading">
      <img src="../assets/loading.gif" alt="loading">
    </div>
    <div class="posts" v-if="!isloading">
      <ul>
        <li v-for="post in posts">
          <!--头像-->
          <img :src="post.author.avatar_url" alt="用户头像">
          <!--回复/浏览-->
          <span class="allcount">
            <span class="reply_count">{{post.reply_count}}</span>/{{post.visit_count}}
          </span>
          <!--帖子的分类-->
          <span :class="[{put_good: (post.good === true),put_top:(post.top === true),
            topiclist_tab:(post.good !== true && post.top !== true)}]">
            {{post | tabFormatter}}
          </span>
          <!--标题-->
          <router-link :to="{name:'post_content',params:{id: post.id,name:post.author.loginname}}">
            {{post.title}}
          </router-link>
          <!--最终回复时间-->
          <span class="last_reply">
            {{post.last_reply_at | formatDate}}
          </span>
        </li>
      </ul>
    </div>
    <Pagination @handleList="renderList"></Pagination>
  </div>
</template>

<script>

  import Pagination from "./Pagination";

  export default {
    name: "PostList",
    data() {
      return {
        isloading: true,
        posts: [],  //代表页面的列表数组
        postPage: 1,
        tab: 'all'
      }
    },
    components: {
      Pagination
    },
    methods: {
      getData() {
        this.$http.get('https://cnodejs.org/api/v1/topics', {
          params: {
            tab: this.tab,
            page: this.postPage,
            limit: 25
          }
        }).then(res => {
          this.loading = false;
          this.posts = res.data.data
        }).catch(err => {
          console.log(err)
        })
      },
      renderList(value) {
        this.postPage = value;
        this.getData()
      }
    },
    beforeMount() {
      this.loading = true;
      this.getData()
    },
    watch: {
      $route() {
        this.tab = this.$route.query.tab
        this.getData()
      }
    }
  }
</script>


<style scoped>
  .loading {
    /*text-align: center;*/
    /*padding-top: 300px;*/
    font-size: 16px;
    background-color: #fff;
    padding: 15px;
    border-radius:  0 0 3px 3px;
    box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.1);
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 60vh;
  }

  .PostList {

  }

  .posts {

  }

  .PostList img {
    height: 30px;
    width: 30px;
    vertical-align: middle;
    border-radius: 3px;
  }

  ul {
    list-style: none;
    width: 100%;
    max-width: 1344px;
    margin: 0 auto;
  }

  ul li {
    padding: 9px;
    font-size: 15px;
    font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
    font-weight: 400;
    background-color: white;
    color: #333;
    border-top: 1px solid #f0f0f0;
  }

  li:hover {
    background: #f5f5f5;;
  }

  li:last-child:hover {
    background: white;
  }

  li span {
    line-height: 30px;
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
    padding: 2px 2px 2px 5px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    color: #fff;
    font-size: 12px;
    margin-right: 10px;
  }

  .topiclist_tab {
    background-color: #e5e5e5;
    color: #999;
    padding: 2px 2px 2px 5px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
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

  .topbar {
    height: 40px;
    background-color: #f6f6f6;
    border-radius: 3px 3px 0 0;
    padding: 0 10px;
  }

  .topbar .topic-tab {
    color: #80bd01;
  }

  .topbar .topic-tab:hover {
    text-decoration: none;
    color: #005580;
  }

  .topbar span {
    font-size: 14px;
    line-height: 40px;
    margin: 0 10px;
    cursor: pointer;
  }

  .topbar .router-link-active {
    background-color: #80bd01;
    color: #fff;
    padding: 3px 4px;
    border-radius: 3px;
  }

  a {
    max-width: 80%;
    white-space: nowrap;
    text-decoration: none;
    color: black;
    text-overflow: ellipsis;
    overflow: hidden;
  }

  a:hover {
    text-decoration: underline;
  }

</style>
