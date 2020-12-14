<template>
  <div>
    <el-dialog :title="info.title" :visible.sync="info.isShow">
      <el-form :model="form">
        <el-form-item label="一级分类" :label-width="width">
          <el-select
            v-model="form.first_cateid"
            placeholder="---请选择---"
            @change="changeCate"
          >
            <!-- 循环添加目录 -->
            <el-option
              v-for="item in cateList"
              :key="item.id"
              :label="item.catename"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="二级分类" :label-width="width">
          <el-select v-model="form.second_cateid" placeholder="---请选择---">
            <!-- 循环添加目录 -->
            <el-option
              v-for="item in secondCate"
              :key="item.id"
              :label="item.catename"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="商品名称" :label-width="width">
          <el-input v-model="form.goodsname" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="价格" :label-width="width">
          <el-input v-model="form.price" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="市场价格" :label-width="width">
          <el-input v-model="form.market_price" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="图片" :label-width="width">
          <el-upload
            class="avatar-uploader"
            action="#"
            :show-file-list="false"
            :on-change="changeImg"
          >
            <img v-if="imageUrl" :src="imageUrl" class="avatar" />
            <i v-else class="el-icon-plus avatar-uploader-icon"></i>
          </el-upload>
        </el-form-item>

        <el-form-item label="商品规格" :label-width="width">
          <el-select
            v-model="form.specsid"
            placeholder="---请选择---"
            @change="changeSpec"
          >
            <!-- 循环添加目录 -->
            <el-option
              v-for="item in specList"
              :key="item.id"
              :label="item.specsname"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="商品属性" :label-width="width">
          <el-select v-model="form.specsattr" placeholder="---请选择---">
            <!-- 循环添加目录 -->
            <el-option
              v-for="item in secondSpec"
              :key="item"
              :label="item"
              :value="item"
            ></el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="是否新品" :label-width="width">
          <el-radio v-model="form.isnew" :label="1">是</el-radio>
          <el-radio v-model="form.isnew" :label="2">否</el-radio>
        </el-form-item>
        <el-form-item label="是否热卖" :label-width="width">
          <el-radio v-model="form.ishot" :label="1">是</el-radio>
          <el-radio v-model="form.ishot" :label="2">否</el-radio>
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

        <el-form-item label="商品描述" :label-width="width">
          <textarea name="" id="" cols="30" rows="10"></textarea>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="info.isShow = false">取 消</el-button>
        <el-button type="primary" @click="add" v-if="info.isAdd"
          >确 定</el-button
        >
        <el-button type="primary" @click="update" v-else>修改</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
import {
  reqgoodsAdd,
  reqgoodsListOne,
  reqgoodsEdit,
} from "../../../util/request";
export default {
  props: ["info"],
  computed: {
    ...mapGetters({
      cateList: "cate/list",
      specList: "specs/list",
    }),
  },
  data() {
    return {
      width: "160px",
      imageUrl: "",
      secondCate: [], //存放二级分类
      secondSpec: [], //用来存放规格属性
      form: {
        first_cateid: "",
        second_cateid: "",
        goodsname: "",
        price: 0,
        market_price: 0,
        img: null,
        specsid: "",
        specsattr: [],
        isnew: 1,
        ishot: 1,
        status: 1,
        description: "",
      },
    };
  },
  methods: {
    empty() {
      this.form = {
        first_cateid: "",
        second_cateid: "",
        goodsname: "",
        price: 0,
        market_price: 0,
        img: null,
        specsid: "",
        specsattr: [],
        isnew: 1,
        ishot: 1,
        status: 1,
        description: "",
      };
    },
    // 获取二级分类
    changeCate() {
      this.secondCate = this.cateList.find((item) => {
        return item.id == this.form.first_cateid;
      }).children;
    },
    // 获取规格属性
    changeSpec() {
      this.secondSpec = this.specList.find((item) => {
        return item.id == this.form.specsid;
      }).attrs;
    },
    // 获取图片
    changeImg(e) {
      console.log(e);
      var file = e.raw;
      this.imageUrl = URL.createObjectURL(file);
      console.log(this.imageUrl);
      this.form.img = file;
    },

    hide() {
      this.info.isShow = false;
    },
    add() {
      reqgoodsAdd(this.form).then((res) => {
        this.empty();
        this.hide();
        this.requestgoodsList();
      });
    },
    ...mapActions({
      requestcateList: "cate/requestcateList",
      requestspecsList: "specs/requestspecsList",
      requestgoodsList: "goods/requestgoodsList",
    }),
    // 获取一条数据
    look(id) {
      reqgoodsListOne({ id: id }).then((res) => {
        this.form = res.data.list;
        this.form.id = id;
        this.imageUrl = this.$preImg + res.data.list.img;
      });
    },
    update() {
      reqgoodsEdit(this.form).then((res) => {
        this.requestgoodsList();
        this.hide();
      });
    },
  },
  mounted() {
    this.requestcateList();
    this.requestspecsList();
  },
};
</script>
<style scoped>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  border: 2px solid #ccc;
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>