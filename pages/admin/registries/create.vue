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
        type="textarea"
        ref="inputValue"
        :class="{'is-drag-enter': isDragEnter}"
        :autosize="{minRows: 5}"
        v-model="inputValue"
        @dragenter.native="isDragEnter = true"
        @dragover.native="isDragEnter = true"
        @dragleave.native="isDragEnter = false"
        @dragend.native="isDragEnter = false"
        @drop.native.prevent="onDrop"
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
      isDragEnter: false,
    }
  },
  methods: {
    async onDrop(e) {
      const files = e.dataTransfer.files
      if (files.length === 0) {
        return
      }
      const form = new FormData()
      form.append("attachment", files[0])
      const response = await this.$axios.post("/attachments", form)
      this.inputValue = `${this.inputValue || ""}${response.data.attachment.path}`
    },
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
