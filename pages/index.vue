<style scoped>
.container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.logo {
  font-size: 3rem;
}
</style>
<template>
  <div class="container">
    <div class="logo">
      <el-button @click="$router.push('/admin')">Go</el-button>
    </div>
  </div>
</template>
