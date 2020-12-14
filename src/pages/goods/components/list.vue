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
      <el-table-column prop="id" label="分类编号" width="80px">
      </el-table-column>
      <el-table-column prop="goodsname" label="商品名称">
      </el-table-column>
      <el-table-column prop="price" label="商品价格">
      </el-table-column>
      <el-table-column prop="market_price" label="市场价格">
      </el-table-column>
      <el-table-column label="图片" width='150px'>
        <template slot-scope="scope">
          <img :src="$preImg + scope.row.img" alt="" class="img1" />
        </template>
      </el-table-column>

      <el-table-column prop="status" label="是否新品">
        <template slot-scope="scope">
          <el-button type="primary" v-if="scope.row.isnew == 1"
            >是</el-button
          >
          <el-button type="danger" v-else>否</el-button>
        </template>
      </el-table-column>
      <el-table-column prop="status" label="是否热卖">
        <template slot-scope="scope">
          <el-button type="primary" v-if="scope.row.ishot == 1"
            >是</el-button
          >
          <el-button type="danger" v-else>否</el-button>
        </template>
      </el-table-column>

      <el-table-column prop="status" label="状态">
        <template slot-scope="scope">
          <el-button type="primary" v-if="scope.row.status == 1"
            >启用</el-button
          >
          <el-button type="danger" v-else>禁用</el-button>
        </template>
      </el-table-column>
      <el-table-column prop="address" label="操作" width='200px'>
        <template slot-scope="scope">
          <el-button type="primary" @click="edit(scope.row.id)">编辑</el-button>
          <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
import { reqgoodsDel } from "../../../util/request";
export default {
  computed: {
    // 获取地址
    ...mapGetters({
      list: "goods/list",
    }),
  },
  methods: {
    // 渲染数据
    ...mapActions({
      requestgoodsList: "goods/requestgoodsList",
    }),
    // 编辑
    edit(id) {
      this.$emit("edit", id);
    },
    // 删除
    del(id) {
      reqgoodsDel({ id: id }).then((res) => {
        alert("删除成功");
        this.requestgoodsList();
      });
    },
  },
  mounted() {
    this.requestgoodsList();
  },
};
</script>
<style scoped>
.box {
  margin-top: 20px;
}
.img1 {
  width: 100%;
}
</style>