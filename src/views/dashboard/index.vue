<template>
  <div class="dashboard-container">
    <div class="title">
      <div class="dashboard-text">Welcome</div>
      <!-- <div class="time">{{ this.nTime }}</div> -->
    </div>
    <div class="splitLine">
      <el-divider content-position="left">数据统计</el-divider>
    </div>
    <div class="Statistics">
      <div class="ArcTotal" @click="handleClick($event)">
        <p>{{ this.ArcTotal }}</p>
        <p>文章数量</p>
      </div>
      <div class="BrowseTotal dColor" @click="handleClick($event)">
        <p>{{ this.BrowseTotal }}</p>
        <p>浏览数量</p>
      </div>
      <div class="CommentTotal" @click="handleClick($event)">
        <p>{{ this.CommentTotal }}</p>
        <p>评论数量</p>
      </div>
      <div class="ProTotal dColor" @click="handleClick($event)">
        <p>{{ this.ProTotal }}</p>
        <p>项目数量</p>
      </div>
    </div>
  </div>
</template>

<script>
import { formatDate } from "@/utils/tools.js";
import { findBlog } from "@/api/blog.js";
import { getBlogType } from "@/api/blogType.js";
import { getProject } from "@/api/project.js";
export default {
  name: "Dashboard",
  data() {
    return {
      timer: "",
      nTime: "",
      ArcTotal: 0,
      BrowseTotal: 0,
      CommentTotal: 0,
      ProTotal: 0,
    };
  },
  methods: {
    getNowTime() {
      var date = new Date();
      return formatDate(date.getTime(), true, true);
    },

    handleClick(e){
      if(e.currentTarget.classList[0] === "ArcTotal"){
       this.$router.push("/bloglist");
      }
       if(e.currentTarget.classList[0] === "BrowseTotal"){
       this.$router.push("/bloglist");
      }
       if(e.currentTarget.classList[0] === "CommentTotal"){
       this.$router.push("/message/index");
      }
       if(e.currentTarget.classList[0] === "ProTotal"){
       this.$router.push("/projectsList");
      }
    }
  },
  created() {
    var _this = this;
    _this.timer = setInterval(() => {
      _this.nTime = this.getNowTime();
    }, 1000);

    getBlogType().then(({ data }) => {
      this.ArcTotal = data.reduce((a, b) => a + b.articleCount, 0);
      findBlog(1, this.ArcTotal).then(({ data }) => {
        this.BrowseTotal = data.rows.reduce((a, b) => a + b.scanNumber, 0);
        this.CommentTotal = data.rows.reduce((a, b) => a + b.commentNumber, 0);
      });
    });

    getProject().then(({data})=>{
      this.ProTotal = data.length;
    })
  },
};
</script>

<style lang="scss" scoped>
.dashboard-container {
  width:100%;
  height:calc(100vh - 50px);
  position: relative;
  padding: 50px;
  text-align: center;
  background: #f2f6fc;
  display: block;
  overflow:scroll;
  .time {
    font-size: 70px;
    margin-top: 50px;
    padding:5px 10px;
    display: inline-block;
    color: #fff;
    border-radius: 5px;
    background: darkseagreen;
  }
  .dashboard-text {
    padding:10px 0;
    font-size: 100px;
    text-align: center;
    color: darkseagreen;
  }
  .splitLine{
    margin:50px 0;
    .el-divider__text{
      background-color:#f2f6fc;
    }
  }
  .Statistics {
    display: flex;
    justify-content: space-between;
    .dColor{
      background: #fff;
      color:#99CCCC;
    }
    div {
      width: 20%;
      border-radius: 5px;
      transition: all 0.5s;
      font-size:22px;
      background: #99CCCC; 
      color:#FFF;
      cursor:pointer;
      box-shadow: 5px 5px 5px #888888;
      :first-child{
        font-size:40px;
        font-weight:bold;
        margin:20px;
      }
      &:hover{
        transform: scale(1.1);
        box-shadow: 10px 10px 5px #888888;
      }
    }
  }
}
</style>
