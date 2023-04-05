<template>
  <div class="container">
    <template v-for="out in pageConfig.list">
      <el-row
        v-if="out.type === 'row'"
        :key="out.columnName"
        :gutter="!out.gutter ? 20 : out.gutter"
      >
        <template v-for="rows in out.list">
          <el-form
            v-if="rows.type === 'form'"
            ref="rows.columnName"
            :model="model"
            :label-width="rows.labelWidth"
            :className="rows.className"
            :key="rows.columnName"
          >
            <template v-for="cols in rows.list">
              <el-col
                v-if="cols.type === 'col'"
                :key="cols.columnName"
                :span="cols.span"
                :align="cols.align"
                :className="cols.className"
              >
                <template v-for="items in cols.list">
                  <el-input
                    v-if="items.type === 'input'"
                    :key="items.columnName"
                    :class="items.className"
                    v-model="model[items.columnName]"
                    placeholder="请输入内容"
                  ></el-input>
                  <el-input
                    v-else-if="items.type === 'textarea'"
                    :type="items.type"
                    :key="items.columnName"
                    :minlength="items.minlength"
                    :maxlength="items.maxlength"
                    :rows="items.rows"
                    v-model="model[items.columnName]"
                    placeholder="请输入内容"
                  ></el-input>
                  <el-select
                    v-else-if="items.type === 'select'"
                    :key="items.columnName"
                    :clearable="items.clearable"
                    v-model="model[items.columnName]"
                    placeholder="请选择"
                  >
                    <el-option
                      v-for="option in items.options"
                      :key="option.value"
                      :label="option.label"
                      :value="option.value"
                    >
                    </el-option>
                  </el-select>
                  <el-button
                    v-else-if="items.type === 'button'"
                    :key="items.columnName"
                    :className="items.className"
                    :type="items.btnType"
                    @click="handleClick({item: items})"
                  >{{ items.label }}</el-button>
                  <p
                    v-else
                    :key="items.columnName"
                  ></p>
                </template>
              </el-col>
            </template>
          </el-form>
          <div
            v-if="rows.type === 'buttonGroup'"
            :className="rows.className"
            :key="rows.columnName"
          >
            <template v-for="cols in rows.list">
              <el-col
                v-if="cols.type === 'col'"
                :key="cols.columnName"
                :span="cols.span"
                :align="cols.align"
                :className="cols.className"
              >
                <template v-for="buttons in cols.list">
                  <el-button
                    :key="buttons.columnName"
                    :className="buttons.className"
                    :type="buttons.btnType"
                    @click="handleClick({item: buttons})"
                  >{{ buttons.label }}</el-button>
                </template>
              </el-col>
            </template>
          </div>
        </template>
      </el-row>
      <template v-if="out.type === 'table'">
        <el-table
          :data="tableData"
          background
          :key="out.columnName"
          @current-change="handleCurrentChange"
        >
          <template v-for="(column, $index) in out.list">
            <el-table-column
              v-if="column.type === 'index'"
              :key="column.columnName"
              :label="column.label"
              :align="column.align"
              :type="column.type"
              :width="column.width"
              :fixed="column.fixed"
            >
            </el-table-column>
            <el-table-column
              v-else-if="column.type === 'selection'"
              :key="column.columnNames"
              :align="column.align"
              :type="column.type"
              :width="column.width"
              :fixed="column.fixed"
            >
            </el-table-column>
            <el-table-column
              v-else-if="column.type === 'operation'"
              :key="column.columnNames"
              :align="column.align"
              :width="column.width"
              :min-width="column.minWidth"
              :fixed="column.fixed"
            >
              <template
                v-for="buttona in column.operation"
                slot-scope="{ row }"
              >
                <el-button
                  :key="buttona.columnName"
                  :className="buttona.className"
                  :type="buttona.type"
                  @click="handleClick({item: buttona, row: row, index: $index})"
                >{{ buttona.label }}</el-button>
              </template>
            </el-table-column>
            <el-table-column
              v-else
              :key="column.columnNamess"
              :prop="column.prop"
              :label="column.label"
              :align="column.align"
              :width="column.width"
              :fixed="column.fixed"
            >
            </el-table-column>
          </template>
        </el-table>
        <!-- 分页区域 -->
        <div
          class="pagination"
          :key="out.columnName"
          v-if="tableConfig.showPage"
        >
          <el-pagination
            :total="total"
            :page-size="pageSize"
            :current-page="currentPage"
          ></el-pagination>
        </div>
      </template>
    </template>
  </div>
</template>

<script>
export default {
  name: 'EditTable',
  props: {
    // 页面配置
    pageConfig: {
      type: Object,
      required: true
    },
    // 表格数据
    tableData: {
      type: Array,
      required: true,
      default: () => []
    },
    // 总记录数
    total: {
      type: Number,
      required: true,
      default: 0
    },
    // 当前页码
    currentPage: {
      type: Number,
      required: true,
      default: 1
    },
    // 每页显示条数
    pageSize: {
      type: Number,
      required: true,
      default: 10
    }
  },
  data () {
    return {
      model: null
    }
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      this.tableConfig = this.pageConfig.tableConfig
      this.model = this.pageConfig.model
    },
    handleClick (data) {
      console.log(data, '按钮点击了。。。')
      this.$emit('afterButton', data)
      // TODO
      this.$emit('beforeButton', data)
    },
    // 处理表格选择项变化事件
    handleSelectionChange (selection) {
      this.selection = selection
      this.$emit('selectionChange', selection)
    },
    // 处理页码变化事件
    handleCurrentChange (currentPage) {
      this.$emit('currentChange', currentPage)
    }
  }
}
</script>

<style lang="less" scoped>
.container {
  padding: 0 15px;
  background: #f7f7f7;
}
</style>