<template>
  <div class="m-content">
    <h1 style="color:white">欢迎来到联通支付技术博客</h1>
    <div class="block">
      <el-avatar :size="50" :src="user.avatar"></el-avatar>
      <div style="color:white">{{ user.username }}</div>
    </div>

    <div class="maction">
      <span><el-link href="/blogs" style="color:white">主页</el-link></span>
      <el-divider direction="vertical"></el-divider>
      <span><el-link style="color:white" href="/blog/add">发表文章</el-link></span>

      <el-divider direction="vertical"></el-divider>
      <span v-show="!hasLogin"><el-link type="primary" href="/login" style="color:white">登录</el-link></span>

      <span v-show="hasLogin"><el-link type="danger" @click="logout" style="color:red">退出登录</el-link></span>
    </div>

  </div>
</template>

<script>
  export default {
    name: "Header",
    data() {
      return {
        user: {
          username: '请先登录',
          avatar: 'https://i.loli.net/2020/09/21/4or5tFQhc71KkUB.png'
        },
        hasLogin: false
      }
    },
    methods: {
      logout() {
        const _this = this
        _this.$axios.get("/logout", {
          headers: {
            "Authorization": localStorage.getItem("token")
          }
        }).then(res => {
          _this.$store.commit("REMOVE_INFO")
          _this.$router.push("/login")

        })
      }
    },
    created() {
      if(this.$store.getters.getUser.username) {
        this.user.username = this.$store.getters.getUser.username
        this.user.avatar = this.$store.getters.getUser.avatar
        this.hasLogin = true
      }

    }
  }
</script>

<style scoped>
  .m-content {
    max-width: 960px;
    margin: 0 auto;
    text-align: center;
  }
  .maction {
    margin: 10px 0;
  }

</style>