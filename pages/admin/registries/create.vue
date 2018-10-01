<template>
  <el-form
    ref="ruleForm2"
    status-icon
    label-width="120px"
  >
    <el-form-item label="Name">
      <el-input ref="inputName" v-model="inputName" type="text" />
    </el-form-item>
    <el-form-item label="Value">
      <el-input
        ref="inputValue"
        :autosize="{minRows: 2}"
        v-model="inputValue" type="textarea"
      />
    </el-form-item>
    <el-form-item>
      <el-button
        type="primary"
        @click="createRegistry"
      >추가하기</el-button>
    </el-form-item>
  </el-form>

</template>
<script>
export default {
  data() {
    return {
      inputName: "",
      inputValue: "",
    }
  },
  methods: {
    async createRegistry() {
      if (!this.inputName) {
        alert("Name을 입력해주세요.")
        return this.$refs.inputName.focus()
      }
      if (!this.inputValue) {
        alert("Value를 입력해주세요.")
        return this.$refs.inputValue.focus()
      }
      
      let value
      try {
        value = JSON.parse(this.inputValue)
      } catch (e) {
        value = this.inputValue
      }
      try {
        await this.$axios.post("/registries", {
          name: this.inputName,
          value,
        })
        this.$router.push("/admin")
      } catch (e) {
        alert(e.message || "처리중 에러가 발생하였습니다.")
      }
    },
  },
}
</script>
