<template>
  <el-form 
        :model="form" 
        ref="form" 
        :rules="rules" 
        class="form">
            <el-form-item class="form-item" prop="username">
                <el-input 
                v-model="form.username"
                placeholder="用户名手机">
                </el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="captcha">
                <el-input 
                v-model="form.captcha"
                placeholder="验证码" >
                    <template slot="append">
                        <el-button @click="handleSendCaptcha">
                            发送验证码
                        </el-button>
                    </template>
                </el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="nickname">
                <el-input 
                v-model="form.nickname"
                placeholder="昵称">
                </el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="password">
                <el-input 
                v-model="form.password"
                placeholder="密码" 
                type="password"
                ></el-input>
            </el-form-item>

            <el-form-item class="form-item" prop="checkPassword">
                <el-input 
                v-model="form.checkPassword"
                placeholder="确认密码" 
                type="password">
                </el-input>
            </el-form-item>

            <!-- <router-link to="@/components/user/loginForm"> -->
            <el-button 
            class="submit" 
            type="primary" 
            @click="handleRegSubmit">
                注册
            </el-button>
            <!-- </router-link> -->

            <!--  <router-link to="/myRegister">注册</router-link> -->
        </el-form>
</template>

<script>
// import loginForm from "@/components/user/loginForm"
// components: {
//     loginForm
// }
export default {
    data () {
        return {
            // 表单数据
            form:{
                username:"",
                nickname:"",
                captcha:"",   //验证码
                password:"",
                checkPassword:"",   //确认密码

            },
            // 表单规则
            rules: {
                username: [
                    { required: true, message: '请输入用户名', trigger: 'blur' }
                ],
                nickname: [
                    { required: true, message: '请输入昵称', trigger: 'blur' }
                ],
                captcha: [
                    { required: true, message: '请输入验证码', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' }
                ],
                checkPassword: [
                    { validator: validatePass, trigger: 'blur' }
                ],
            },
        }

        // 确认密码的验证
        // rule：当前的规则，一般用不上这个参数
        // value：输入框的值
        // callback：回调函数，该函数必须要调用，调用的时候可以传递错误的对象信息

        var validatePass = (rule,value,callback)=>{
            if(value === ""){
                // new Error js 原生的错误对象
                callback(new Error("小老弟，还要再输入一遍哦"));

            } else if (value !== this.form.password) {
                callback(new Error('老哥，错了'));
            } else {
                // 验证通过
                callback();
            }
        }
    },
    methods:{
        // 发送验证码
        handleSendCaptcha(){
if(!this.form.username){
        this.$confirm('手机号码不能为空', '提示', {
            confirmButtonText: '确定',
            showCancelButton: false,
            type: 'warning'
        })
        return;
    }

    if(this.form.username.length !== 11){
        this.$confirm('手机号码格式错误', '提示', {
            confirmButtonText: '确定',
            showCancelButton: false,
            type: 'warning'
        })
        return;
    }


    this.$axios({
        url: `/captchas`,
        method: "POST",
        data: {
            tel: this.form.username
        }
    }).then(res => {
        const {code} = res.data;
        this.$confirm(`模拟手机验证码为：${code}`, '提示', {
            confirmButtonText: '确定',
            showCancelButton: false,
            type: 'warning'
        })
    })
        },

        // 注册
        handleRegSubmit(){
          this.$refs.form.validate(async valid => {
              if(valid){
                  
                  // props是form里面除了checkPassword以外的属性
                  const {checkPassword, ...props} = this.form

                  // 请求注册的接口
                  const res = await this.$axios({
                      url: "/accounts/register",
                      method: "POST",
                      data: props
                  })

                  if(res.status === 200){
					   this.$message.success("注册成功");

					   // 跳转到首页
                       this.$router.push("/")
                    
					   const data = res.data;
					   
                       // 把用户信息token保存到本地，在头部组件中显示用户数据
                       this.$store.commit("user/setUserInfo", data);
                       
                   }
              }
          })
        }

    },




}
</script>

<style scoped lang="less">
    .form{
        padding:25px;
    }

    .form-item{
        margin-bottom:20px;
    }

    .form-text{
        font-size:12px;
        color:#409EFF;
        text-align: right;
        line-height: 1;
    }

    .submit{
        width:100%;
        margin-top:10px;
    }
</style>