<template>
    <el-table
    v-bind="tableAttr"
    :data="data"
    v-on="events"
    style="width: 100%">
      <template v-for="(column,index) in columns" >
        <el-table-column v-if="column.slotName" v-bind="column" >
          <template slot-scope="scope">
            <slot :name="column.slotName" :column="column" :$index="scope.$index" :options="column.options" :row="scope.row"/>
          </template>
        </el-table-column>
        <el-table-column v-else v-bind="column" >
        </el-table-column>
      </template>
    </el-table>
</template>
<script>
export default {
  props: {
    columns: { type: Array, required: true },
    data: { type: Array, required: true },
    tableAttr: { type: Object },
    events: Object,
    pagination: Object // TODO:
  },
  data() {
    return {};
  },
  methods: {
    setKey(column, index) {
      let key = column.prop || column["column-key"] || column.key || index;
      return key;
    }
  },
  created() {},
  components: {}
};
</script>