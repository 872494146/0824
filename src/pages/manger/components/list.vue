<template>
  <div class="box">
    <el-table
      :data="list"
      style="width: 100%; margin-bottom: 20px"
      row-key="id"
      border
      default-expand-all
      :tree-props="{ children: 'children', hasChildren: 'hasChildren' }"
    >
      <el-table-column prop="id" label="用户编号" sortable width="180">
      </el-table-column>
      <el-table-column prop="username" label="用户名称" sortable width="180">
      </el-table-column>
      <el-table-column prop="rolename" label="所属角色" sortable width="180">
      </el-table-column>

      <el-table-column prop="status" label="状态">
        <template slot-scope="scope">
          <el-button type="primary" v-if="scope.row.status == 1"
            >启用</el-button
          >
          <el-button type="danger" v-else>禁用</el-button>
        </template>
      </el-table-column>
      <el-table-column prop="address" label="操作">
        <template slot-scope="scope">
          <el-button type="primary" @click="edit(scope.row.uid)">编辑</el-button>
          <el-button type="danger" @click="del(scope.row.uid)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      :current-page="page"
      :page-size="size"
      layout="prev, pager, next"
      :total="total"
      @current-change="changeCurrentPage"
    >
    </el-pagination>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
import { requserDel } from "../../../util/request";
export default {
  computed: {
    // 获取地址
    ...mapGetters({
      list: "manger/list",
      total: "manger/total",//请求回来的总数
      page: "manger/page", //当前第几页
      size: "manger/size" //每页展示的条数
    }),
  },
  methods: {
    // 渲染数据
    ...mapActions({
      requestuserList: "manger/requestuserList",
      requestuserCount:'manger/requestuserCount',
      changeCurrentPages:'manger/changeCurrentPages'
    }),
    // 修改当前页面
    changeCurrentPage(p){
        this.changeCurrentPages(p)
    },
    // 编辑
    edit(id) {
      this.$emit("edit", id);
    },
    // 删除
    del(id) {
      requserDel({ uid: id }).then((res) => {
        alert("删除成功");
        this.requestuserList()
        this.requestuserCount()
        this.changeCurrentPages(1)
      });
    },
  },
  mounted() {
    this.requestuserList();
    this.requestuserCount()
  },
};
</script>
<style scoped>
.box {
  margin-top: 20px;
}
</style>