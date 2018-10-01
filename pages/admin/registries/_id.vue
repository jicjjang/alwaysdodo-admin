<template>
  <el-form
    ref="ruleForm2"
    status-icon
    label-width="120px"
  >
    <el-form-item label="Name">
      <el-input v-model="inputName" type="text" ref="inputName" />
    </el-form-item>
    <el-form-item label="Value">
      <el-input :autosize="{minRows: 2}" v-model="inputValue" type="textarea" ref="inputValue" />
    </el-form-item>
    <el-form-item>
      <div class="align-justify">
        <el-button type="primary" @click="updateRegistry">수정하기</el-button>
        <el-button type="danger" icon="el-icon-delete" circle @click="deleteRegistry" />
      </div>
    </el-form-item>
  </el-form>

</template>
<script>
export default {
  async asyncData({app: {$axios}, params: {id}, error}) {
    const response = await $axios.get("/registries")
    id = +id
    const registry = response.data.registries.find((registry) => {
      return registry.id === id
    })
    if (!registry) {
      error({ statusCode: 404, message: 'Post not found' })
      return
    }
    const inputValue = typeof registry.value === "string" ? registry.value : JSON.stringify(registry.value, null, "  ")
    return {
      inputName: registry.name,
      inputValue,
    }
  },
  methods: {
    async updateRegistry() {
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
        await this.$axios.put(`/registries/${this.$route.params.id}`, {
          name: this.inputName,
          value,
        })
        this.$router.push("/admin")
      } catch (e) {
        alert(e.message || "처리중 에러가 발생하였습니다.")
      }
    },
    async deleteRegistry() {
      if (!confirm("정말로 삭제하시겠습니까?")) {
        return
      }
      try {
        await this.$axios.delete(`/registries/${this.$route.params.id}`)
        this.$router.push("/admin")
      } catch (e) {
        alert(e.message || "처리중 에러가 발생하였습니다.")
      }
    },
  },
}
</script>
