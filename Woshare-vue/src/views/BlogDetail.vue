<template>
  <div class="mcontaner">
    <Header></Header>
    <div class="mblog">
      <div class="mblog_titile">
        <h2 style="margin:0 20 0 30">{{ blog.title }}</h2>
        <router-link :to="{name: 'BlogEdit', params: {blogId: blog.id}}">
          <el-button type="primary" icon="el-icon-edit">编辑文章</el-button>
        </router-link>
      </div>
      <div class="markdown-body" v-html="blog.content"></div>
    </div>
  </div>
</template>

<script>
import "github-markdown-css";
import Header from "../components/Header";

export default {
  name: "BlogDetail.vue",
  components: { Header },
  data() {
    return {
      blog: {
        id: "",
        title: "",
        content: "",
      },
      ownBlog: false,
    };
  },
  created() {
    const blogId = this.$route.params.blogId;
    console.log(blogId);
    const _this = this;
    this.$axios.get("/blog/" + blogId).then((res) => {
      const blog = res.data.data;
      _this.blog.id = blog.id;
      _this.blog.title = blog.title;

      var MardownIt = require("markdown-it");
      var md = new MardownIt();

      var result = md.render(blog.content);
      _this.blog.content = result;
      _this.ownBlog = blog.userId === _this.$store.getters.getUser.id;
    });
  },
};
</script>

<style scoped>
.mcontaner {
  margin-top: -22px;
  padding-top: 30px;
  padding-bottom: 15px;
  background-image: url("../assets/backgImg.jpg");
}
.mblog {
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  width: 100%;
  min-height: 700px;
  padding: 20px 50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.markdown-body {
  margin: 0 auto;
  max-width: 75%;
  font-size: 20px;
  font-family: "Courier New", Courier, monospace;
}

.mblog_titile{
  max-width: 90%;
  display:flex;
  justify-content: space-between;
  align-items: center;
}
.el-button--primary {
  background: hsl(11, 79%, 50%);
  border: none;
  color: #fff;
}
</style>