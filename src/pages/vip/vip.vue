<template>
  <div class="box">
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
        title: "会员修改",
        isAdd: true,
      },
    };
  },
  methods: {
    edit(e) {
      this.info.isShow = true;
      this.info.title = "规格修改";
      this.info.isAdd = false;
      this.$refs.one.look(e);
    },
  },
  mounted() {},
};
</script>
<style scoped>
.box {
  margin-top: 20px;
}
</style>