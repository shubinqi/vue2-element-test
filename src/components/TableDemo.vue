<template>
  <div>
    <edit-table
      :table-data="tableData"
      :model="model"
      :pageConfig="pageConfig"
      :pageSize="tableConfig.pagination.pageSize"
      :pageSizes="tableConfig.pagination.pageSizes"
      :total="tableConfig.pagination.total"
      :currentPage="tableConfig.pagination.currentPage"
    ></edit-table>
  </div>
</template>

<script>
import EditTable from './EditTable.vue'
import { pageConfig } from './pageConfig'

export default {
  components: {
    EditTable
  },
  name: 'TableDemo',
  data () {
    return {
      tableData: [{
        prop1: '123',
        prop2: '123',
        prop3: '123'
      }, {
        prop1: '1231',
        prop2: '1231',
        prop3: '1231'
      }, {
        prop1: '12311',
        prop2: '12311',
        prop3: '12311'
      }],
      tableConfig: null,
      model: null
    }
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      console.log(pageConfig, 'this.pageConfig')
      this.pageConfig = pageConfig
      this.model = pageConfig.model
      this.tableConfig = pageConfig.tableConfig
    }
  }
}
</script>

<style lang="less" scoped>
</style>