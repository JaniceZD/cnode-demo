<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="judge" class="pagebtn">...</button>
    <button v-for="btn in pagebtns"
            @click="changeBtn(btn)"
            :class="[{currentPage: btn === currentPage},'pagebtn']">
      {{btn}}
    </button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
  import $ from 'jquery'

  export default {
    name: "Pagination",
    data() {
      return {
        pagebtns: [1, 2, 3, 4, 5, '...'],
        currentPage: 1,  //当前选中的页码
        judge: false  //判断页码是否大于4
      }
    },
    methods: {
      changeBtn(page) {
        if (typeof page !== 'number') {
          switch (page.target.innerText) {
            case '上一页':
              $('button.currentPage').prev().click()
              break
            case '下一页':
              $('button.currentPage').next().click()
              break
            case '首页':
              this.pagebtns = [1, 2, 3, 4, 5, '...']
              this.changeBtn(1)
              break
            default:
              break;
          }
          return;
        }
        this.currentPage = page
        this.judge = page > 4 ? true : false
        //页码大于4时
        if (page === this.pagebtns[4]
        ) {
          this.pagebtns.shift(); //移除第一个元素
          this.pagebtns.splice(4, 0, this.pagebtns[3] + 1) //添加最后一个
        } else if (page === this.pagebtns[0] && page !== 1) {
          this.pagebtns.unshift(this.pagebtns[0] - 1)
          this.pagebtns.splice(5, 1)
        }
        this.$emit('handleList', this.currentPage)
      }
    }
  }
</script>

<style scoped>
  .pagination {
    margin-top: 5px;
    margin-bottom: 6px;
    background-color: white;
    padding: 6px 4px;
    border-radius: 3px;
    /*box-shadow: 0px 2px 9px #888888;*/
  }

  button {
    background-color: #f6f6f6;
    border: 1px solid #ddd;
    color: #778087;
    border-radius: 3px;
    outline: none;
    padding: 0 2px;
    width: 55px;
    height: 29px;
    cursor: pointer;
  }

  .pagebtn {
    position: relative;
    bottom: 1px;
    width: 40px;
    margin: 0 4px;
  }

  .currentPage {
    background-color: #80bd01;
    color: #fff;

  }
</style>
