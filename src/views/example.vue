<template>
    <div class="">
      <el-main>
      <vue-el-table :data="tableData" :el-table-attr="{}" :el-table-events="{'row-click':rowClick}" :columns="tableColumns">
        <div slot="date" slot-scope="scope">
          <i class="el-icon-time"></i>
          <span style="margin-left: 10px">{{ scope.row.date }}</span>
        </div>
        <div slot="oper" slot-scope="scope">
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)">删除
          </el-button>
        </div>
        <div slot="model" slot-scope="{row}">
          <el-input v-model="row.name"></el-input>
        </div>
        <div slot="expand" slot-scope="scope">
          <div >
            <span>日期：</span>
            <span>{{ scope.row.date }}</span>
          </div>
          <div >
            <span>姓名：</span>
            <span>{{ scope.row.name }}</span>
          </div>
          <div >
            <span>地址：</span>
            <span>{{ scope.row.address }}</span>
          </div>
        </div>
      </vue-el-table>
      </el-main>      
    </div>
</template>
<script>
import vueElTable from '@/components/vue-el-table.vue';
export default {
  data() {
    return {
      tableData: [
        {
          date: "2016-05-02",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1518 弄"
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄"
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄"
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄"
        }
      ],
      tableAttr: { "highlight-current-row": true, stripe: true },
      rowClickNum: 0,
      tableColumns: [
        {
          type: "expand",
          slotName: "expand"
        },
        {
          type: "selection",
        },
        {
          type: "index",
          label: "序号",
          align: "center",
          width: "50"
        },
        {
          prop: "date",
          label: "日期",
          slotName: "date",
          seq:6
        },
        {
          prop: "name",
          label: "姓名"
        },
        {
          prop: "nameFilter",
          label: "姓名[过滤]",
          render:(h,{row})=>{
            return(
              <div>{ this.$options.filters.nameFilter(row.name)}</div>
            )
          }
        },
        {
          prop: "address",
          label: "地址"
        },
        {
          prop: "edit",
          label: "编辑",
          width: "200",
          slotName:'model',
        },
        {
          prop: "oper",
          label: "操作",
          slotName: "oper",
          render:(h,scope)=>{
            return (
              <div>
                <el-button
                  size="mini"
                  type="danger"
                  onClick={()=>this.handleDelete(scope.$index, scope.row)}>删除
                </el-button>
              </div>
            )
          }
        }
      ]
    };
  },
  methods: {
    rowClick(row, _, column) {
      console.log(`第 ${++this.rowClickNum} 次单击行`);
    },
    handleDelete(index, row) {
      this.$confirm("此操作不可恢复，确认删除?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.tableData.splice(index, 1);
          this.$message({
            type: "success",
            message: "删除成功!"
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除!"
          });
        });
    }
  },
  filters:{
    nameFilter(name){
      return '[已通过]' + name
    }
  },
  components: {vueElTable},
  created() {},
  mounted(){
  }
};
</script>
<style scoped>
</style>