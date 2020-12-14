<template>
  <div class="box">
    <div class="con">
      <h3>登录</h3>
      <div class="inp"><input type="text" v-model="user.username" /></div>
      <div class="inp"><input type="text" v-model="user.password" /></div>
      <div class="inp"><button @click="login">登录</button></div>
    </div>
  </div>
</template>
<script>
import {requserLogin} from "../../util/request"
import { mapActions, mapGetters } from "vuex";
export default {
  data() {
    return {
      user: {
        username: "",
        password: "",
      },
    };
  },
  methods: {
    ...mapActions({
        requestuserList:'user/requestuserList'
    }),
    login() {
        requserLogin(this.user).then((res)=>{
            if(res.data.code == 200){
                this.requestuserList(res.data.list);
                this.$router.push("./index/home");
            }else{
                alert(res.data.msg)
            }
        })
    },
  },
};
</script>
<style  scoped>
.box {
  width: 100vw;
  height: 100vh;
  background: linear-gradient(to right, #563443, #303d60);
}
.con {
  width: 350px;
  height: 250px;
  background: #fff;
  position: relative;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 10px;
}
.con h3 {
  text-align: center;
}
.con .inp {
  width: 60%;
  height: 30px;
  margin: 25px auto;
}
.con input {
  width: 100%;
  height: 30px;
}
.inp button {
  width: 100%;
  height: 30px;
  background-color: #409eff;
  color: #fff;
  outline: none;
  border: none;
}
</style> 