<template>
  <div class="box">
    <el-button type="primary" @click="willAdd">添加</el-button>
    <v-list @edit="edit($event)"></v-list>
    <v-add :info="info" ref="one"></v-add>
  </div>
</template>
<script>
import vList from "./components/list";
import vAdd from "./components/add";
export default {
  components: {
    vList,
    vAdd,
  },
  data() {
    return {
      info: {
        isShow: false,
        title: "角色添加",
        isAdd: true,
      },
    };
  },
  methods: {
    willAdd() {
      this.info.isShow = !this.info.isShow;
      (this.info.title = "角色添加"), (this.info.isAdd = true);
      this.$refs.one.empty();
    },
    // 编辑
    edit(id) {
      this.info.isShow = true;
      (this.info.title = "角色修改"), (this.info.isAdd = false);
      this.$refs.one.look(id);
    },
  },
};
</script>
<style scoped>
.box{
  margin-top: 20px;
}
</style>
