<style scoped>
  .type {
    display: inline-block;
    padding: .25em .4em;
    font-size: 75%;
    line-height: 1;
    border-radius: .25rem;
  }
  .type-string {
    background-color: #ffc107;
  }
  .type-number {
    background-color: #17a2b8;
    color: #fff;
  }
  .type-object {
    background-color: #007bff;
    color: #fff;
  }
  .type-boolean {
    background-color: #dc3545;
    color: #fff;
  }
  .type-array {
    background-color: #28a745;
    color: #fff;
  }
</style>
<template>
  <div>
    <el-button
      type="primary"
      icon="el-icon-edit"
      circle
      @click="$router.push('/admin/registries/create')"
    />
    <el-table :data="registries">
      <el-table-column prop="id" label="ID" width="60" />
      <el-table-column prop="name" label="Name" width="120" />
      <el-table-column label="Type" width="80">
        <template slot-scope="scope">
          <span class="type" :class="[`type-${getType(scope.row.value)}`]">{{ getType(scope.row.value) }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Value">
        <code slot-scope="scope"><pre>{{ formatJson(scope.row.value) }}</pre></code>
      </el-table-column>
      <el-table-column fixed="right" width="120">
        <template slot-scope="scope">
          <el-button size="small" type="text" @click="$router.push(`/admin/registries/${scope.row.id}`)">Edit</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  async asyncData({app: {$axios}}) {
    const response = await $axios.get("/registries")
    return {
      registries: response.data.registries,
    }
  },
  methods: {
    getType(obj) {
      if (Array.isArray(obj)) {
        return "array"
      }
      return typeof obj
    },
    formatJson(data) {
      return JSON.stringify(data, null, "  ")
    },
  },
}
</script>
