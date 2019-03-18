<template>
    <el-table
    v-bind="$attrs"
    :data="data"
    ref="elTable"
    v-on="$listeners">
      <template v-for="(column,index) in computedColumns" >
        <el-table-column :key="setKey(column,index)" v-if="column.render||column.slotName" v-bind="column">
          <template slot-scope="scope">
            <extend-dom v-if="column.render" :column="column" :$index="scope.$index" :options="column.options" :row="scope.row"/>
            <slot v-else :name="column.slotName" :column="column" :$index="scope.$index" :options="column.options" :row="scope.row"/>
          </template>
        </el-table-column>
        <el-table-column :key="setKey(column,index)" v-else v-bind="column" :formatter="column.formatter || cellFormatter">
        </el-table-column>
      </template>
      <div slot="append" >
        <slot name="append" />
      </div>
    </el-table>
</template>
<script>
// 优势：100兼容原table组件。
// 宗旨：让相同的table-column可以得到最便捷、最极致的复用。
// 1. 支持jsx
// 2. 支持插槽
// 3. 支持自定义顺序
// 4. 支持原table任意属性和事件、支持原table-column任意属性。
// 5. 支持原table方法。

// 难点：
// 1. 绑定原组件this。

// 注意点：
// 1. jsx 要是用到组件实例this，需要自己维护。比如使用箭头函数。
// 2. jsx 使用 v-model，请安装babel-plugin-jsx-v-model
// 3. filter请使用this.$options.filters.xxx()的形式调用
import { Table } from 'element-ui'

const getType = function(obj){
  return Object.prototype.toString.call(obj).slice(8, -1);
}
export default {
  inheritAttrs:false,
  props: {
    columns: { type: Array, required: true },
    data: { type: Array | null, required: true },
    elTableAttrs: { type: Object },
    pagination: Object // TODO:
  },
  data() {
    return {};
  },
  computed:{
    computedColumns(){
      let columns = [...this.columns]
      let seqColumns = columns.filter(item=>{
        return typeof item.seq === 'number'
      })
      let newSeqedColumns = new Array(columns.length)
      let newColIndex = 0
      for (const item of columns) {
        if(getType(item) !== 'Object'){
          throw {name:'TypeError',message:'table columns option is not an object'}
        }
        if(seqColumns.includes(item)){
          newSeqedColumns[item.seq] = item
        }else{
          while(newSeqedColumns[newColIndex]){
            newColIndex ++ // 跳过
          }
          newSeqedColumns[newColIndex++] = item
        }
      }
      return newSeqedColumns
    }
  },
  methods: {
    cellFormatter(row,c,cellValue){
      if(cellValue == undefined || cellValue === ""){
        return '-'
      }
      return cellValue
    },
    setKey(column, index) {
      let key = column.prop || column["column-key"] || column.key || index;
      return key;
    }
  },
  created() {
  },
  mounted(){
    this.$nextTick(() => {
      // proxy
      Object.keys(Table.methods).forEach((item) => {
        this[item] = this.$refs.elTable[item]
      })
    })
  },
  components: {
    extendDom:{
      functional:true,
      render(h,ctx){
        let params = {...ctx.props}
        return ctx.props.column.render.call(null,h,params)
      }
    }
  }
};
</script>