<template>
  <el-table :data="registries">
    <el-table-column prop="id" label="ID" width="140" />
    <el-table-column prop="name" label="Name" width="120" />
    <el-table-column prop="address" label="Value" />
  </el-table>
</template>
<script>
export default {
  data() {
    return {
      registries: [],
    }
  },
}
</script>
