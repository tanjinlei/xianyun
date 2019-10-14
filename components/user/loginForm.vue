<template>
  <el-form :model="form" ref="form" :rules="rules" class="form">
    <!-- 用户名 -->
    <el-form-item class="form-item" prop="username">
      <el-input v-model="form.username" placeholder="用户名/手机"></el-input>
    </el-form-item>

    <!-- 密码 -->
    <el-form-item class="form-item" prop="password">
      <el-input placeholder="密码" v-model="form.password" type="password"></el-input>
    </el-form-item>
    <p class="form-text">
      <nuxt-link to="#">忘记密码</nuxt-link>
    </p>

    <!-- 登录按钮 -->
    <el-button class="submit" type="primary" @click="hanleLoginSubmit">登录</el-button>
  </el-form>
</template>

<script>
import { async } from "q";
export default {
  data() {
    return {
      // 表单数据
      form: {
        username: "",
        password: ""
      },
      // 表单规则
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }]
      }
    };
  },
  methods: {
    // 提交登录
    hanleLoginSubmit() {
      this.$refs.form.validate(async valid => {
        console.log(valid);
        if (valid) {
          var res = await this.$axios({
            url: "/accounts/login",
            method: "POST",
            data: this.form
          });

          if (res.status === 200) {
            this.$message.success("登录成功");

            this.$router.push("/")

            const data = res.data;
            // 把用户信息token保存到本地，在头部组件中显示用户数据
            // vuex不能通过直接复制方式来修改state的值
            // this.$store.state.user.username = data.user.nickname;
            // 通过调用mutation下的方法修改state的值，commit方法调用mutation的方法
            // 非常类似于$store
            this.$store.commit("user/setUserInfo",data)

          }
        }
      });
    }
  }
};
</script>

<style scoped lang="less">
.form {
  padding: 25px;
}

.form-item {
  margin-bottom: 20px;
}

.form-text {
  font-size: 12px;
  color: #409eff;
  text-align: right;
  line-height: 1;
}

.submit {
  width: 100%;
  margin-top: 10px;
}
</style>