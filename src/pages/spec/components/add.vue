<template>
  <div>
    <el-dialog :title="info.title" :visible.sync="info.isShow">
      <el-form :model="form">
        <el-form-item label="规格名称" :label-width="width">
          <el-input v-model="form.specsname" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item
          label="规格属性"
          :label-width="width"
          v-for="(item, index) in arrAttr"
          :key="index"
        >
          <el-col :span="17">
            <el-input v-model="item.value" autocomplete="off"></el-input>
          </el-col>
          <el-col :span="3">
            <el-button v-if="index == 0" type="primary" @click="addAttr"
              >新增规格属性</el-button
            >
            <el-button v-else type="danger" @click="delAttr(index)">删除</el-button>
          </el-col>
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
        <el-button type="primary" @click="add" v-if="info.isAdd"
          >添 加</el-button
        >
        <el-button type="primary" @click="update" v-else>修 改</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
import { reqspecsAdd, reqspecsEdit, reqspecsListOne}from '../../../util/request'
export default {
  props: ["info"],
  components: {
    //   ...mapGetters({
    //       cateList:"cate/list"
    //   })
  },
  data() {
    return {
      width: "160px",
      form: {
        specsname: "",
        attrs: "",
        status: 1,
      },
      arrAttr: [{ value: "" }],
    };
  },
  methods: {
    ...mapActions({
       requestcateList:'cate/requestcateList' 
    }),
      // 重置
    empty() {
      this.form = {
        specsname: "",
        attrs: "",
        status: 1,
      };
      this.arrAttr = [{value: ''}]
    },
    // 隐藏弹框
    hide() {
      this.info.isShow = false;
    },
      //添加
    add() {
        this.form.attrs = JSON.stringify(this.arrAttr.map(item=>{return item.value}))
        reqspecsAdd(this.form).then((res)=>{
            this.empty();
            this.hide();
        })
    },
    // 修改
    update() {
        this.form.attrs = JSON.stringify(this.arrAttr.map(item=>{return item.value}))
        reqspecsEdit(this.form).then((res)=>{
            this.requestcateList();
            this.hide();
        })
    },
    // 查看
    look(id){
        reqspecsListOne({id:id}).then((res)=>{
            this.form = res.data.list[0]
            this.form.id  = id;
            this.arrAttr =JSON.parse(this.form.attrs).map(item=>{return {value:item}})
        })
    },
    // 新增属性
    addAttr() {
        this.arrAttr.push({
            value:''
        })
    },
    // 删除属性
    delAttr(index) {
        this.arrAttr.splice(index,1)
    },
  },
  mounted() {
      this.requestcateList()
  },
};
</script>
<style scoped>
</style>