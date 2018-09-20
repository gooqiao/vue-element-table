<template>
    <div class="">
      <common-table :data="tableData" :table-attr="{}" :events="{'row-click':rowClick}" :columns="tableColumns">
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
      </common-table>
    </div>
</template>
<script>
import { CommonTableMixin } from "@/assets/js/vueCommon.js";
export default {
  mixins: [CommonTableMixin],
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
          type: "index",
          label: "序号",
          align: "center",
          width: "50"
        },
        {
          prop: "date",
          label: "日期",
          slotName: "date"
        },
        {
          prop: "name",
          label: "姓名"
        },
        {
          prop: "address",
          label: "地址"
        },
        {
          prop: "oper",
          label: "操作",
          slotName: "oper"
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
  components: {},
  created() {}
};
</script>
<style lang="stylus" scoped>
</style>