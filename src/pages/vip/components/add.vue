<template>
  <div>
    <el-dialog :title="info.title" :visible.sync="info.isShow">
      <el-form :model="form">
        <el-form-item label="手机号" :label-width="width">
          <el-input v-model="form.phone" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="昵称" :label-width="width">
          <el-input v-model="form.nickname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="width">
          <el-input v-model="form.password" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="状态" :label-width="width">
          <el-switch
            v-model="form.status"
            active-color="#13ce66"
            inactive-color="#ff4949"
            :active-value="1"
            :inactive-value="2"
          >
          </el-switch>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="info.isShow = false">取 消</el-button>
        <el-button type="primary" @click="update">修 改</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
import { reqvipEdit, reqvipListOne}from '../../../util/request'
export default {
  props: ["info"],
  components: {
      ...mapGetters({
          vipList:"vip/list"
      })
  },
  data() {
    return {
      width: "160px",
      form: {
        uid: "",
        nickname: "",
        phone: "",
        password: "",
        status: 1,
      },
    };
  },
  methods: {
    ...mapActions({
       requestcateList:'cate/requestcateList' 
    }),
    // 隐藏弹框
    hide() {
      this.info.isShow = false;
    },
    // 修改
    update() {
        reqvipEdit(this.form).then((res)=>{
            this.requestcateList();
            this.hide();
        })
    },
    // 查看
    look(id){
        reqvipListOne({uid:id}).then((res)=>{
            console.log(res);
            this.form = res.data.list
            this.form.id  = id;
        })
    },
  },
  mounted() {
      this.requestcateList()
  },
};
</script>
<style scoped>
</style>