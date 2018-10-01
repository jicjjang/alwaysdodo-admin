<template>
  <div class="vertical-center height-full">
    <el-form
      ref="ruleForm2"
      status-icon
      label-width="120px"
      @submit="onSubmit"
    >
      <el-form-item label="Username">
        <el-input v-model="inputUsername" type="text" />
      </el-form-item>
      <el-form-item label="Password">
        <el-input
          v-model="inputPassword"
          type="password"
          autocomplete="off"
        />
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          @click="onSubmit"
        >Submit</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputUsername: null,
      inputPassword: null,
    }
  },
  methods: {
    async onSubmit() {
      const data = {
        username: this.inputUsername,
        password: this.inputPassword,
      }
      try {
        await this.$auth.login({data})
        this.$router.push("/admin")
      } catch (e) {
        alert(e.message || "처리중 에러가 발생하였습니다.")
      }
    }
  },
}
</script>
