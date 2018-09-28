<template>
  <div class="vertical-center height-full">
    <el-form
      ref="ruleForm2"
      status-icon
      label-width="120px"
    >
      <el-form-item label="Username">
        <el-input
          type="text"
        />
      </el-form-item>
      <el-form-item label="Password">
        <el-input
          type="password"
          autocomplete="off"
        />
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          @click="submitForm('ruleForm2')"
        >Submit</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      inputEmail: null,
      inputPassword: null,
      errors: [],
    }
  },
  methods: {
    async onSubmit() {
      const data = {
        email: this.inputEmail,
        password: this.inputPassword,
      }
      try {
        await this.$auth.login({data})
        this.$router.push("/admin")
      } catch (e) {
        const response = e.response.data
        this.errors = []
        if (response.message) {
          this.errors.push(response.message)
        }
        for (const error of response.errors || []) {
          this.errors.push(error.message)
        }
      }
    }
  },
}
</script>
