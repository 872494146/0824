<template>
  <div class="box">
    <el-button type="primary" @click="willAdd">添加</el-button>
    <v-add :info="info" ref="one"></v-add>
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>
<script>
import vAdd from "./components/add";
import vList from "./components/list";
export default {
  components: {
    vAdd,
    vList,
  },
  data() {
    return {
      info: {
        isShow: false,
        title: "商品管理添加",
        isAdd: true,
      },
    };
  },
  methods: {
    willAdd() {
      this.info.isShow = !this.info.isShow;
      this.info.title = "商品管理添加";
      this.info.isAdd = true;
      this.$refs.one.empty();
    },
    // 编辑
    edit(id) {
      this.info.isShow = true;
      this.info.title = "商品管理修改";
      this.info.isAdd = false;
      this.$refs.one.look(id);
    },
  },
};
</script>
<style scoped>
.box {
  margin-top: 20px;
}
</style>