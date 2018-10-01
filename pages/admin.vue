<template>
  <el-container class="height-full">
    <el-header>
      <nuxt-link to="/admin" class="thumb">DODO</nuxt-link>
      <el-button type="info" @click="logout">로그아웃</el-button>
    </el-header>
    <el-container>
      <el-aside width="200px">
        <el-menu style="height: 100%" router>
          <el-menu-item index="/admin">
            <i class="el-icon-menu" />
            <span slot="title">설정값</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-main>
        <nuxt-child />
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
export default {
  middleware: [
    "auth",
  ],
  methods: {
    logout() {
      this.$auth.logout()
    },
  },
}
</script>
