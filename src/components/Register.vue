<template>
  <div class="login-container" style="background-color: #141a48;margin: 0px;overflow: hidden;">
    <div id="canvascontainer" ref='can'></div>
    <Form ref="registerForm" :model="registerForm" :rules="registerRules" class="card-box login-form">
      <Form-item label="账户" prop="mobile">
        <Input type="text" v-model="registerForm.mobile" placeholder="请输入手机号" autoComplete="off">
        </Input>
      </Form-item>
      <Form-item label="密码" prop="password">
        <Input type="password" v-model="registerForm.password" placeholder="请输入5-8位密码" autoComplete="off">
        </Input>
      </Form-item>
      <Form-item label="昵称" prop="nickName">
        <Input type="text" v-model="registerForm.nickName" placeholder="请输入2-8个字符的昵称">
        </Input>
      </Form-item>
      <Form-item label="性别">
        <RadioGroup v-model="registerForm.sex" size='large'>
          <Radio label="MALE" style="padding-left:50px;">
            <Icon type="male" color='#6DA6C6'></Icon>
            <span>男</span>
          </Radio>
          <Radio label="FEMALE" style="padding-left:50px;">
            <Icon type="female" color='#CF6D69'></Icon>
            <span>女</span>
          </Radio>
        </RadioGroup>
      </Form-item>
       <Form-item>
         <Button type="text"  @click="goLogin('')" long>已有账号去登录</Button>
      </Form-item>
      <Form-item>
        <Button type="primary" @click="handleRegister('registerForm')" long>注册</Button>
      </Form-item>
    </Form>
  </div>
</template>

<script>
  import {
    isMobile
  } from "../utils/validate"
  import {
    register
  } from '@/resources/user'
  export default {
    data() {
      const validateMobile = (rule, value, callback) => {
        if (!isMobile(value)) {
          callback(new Error("请输入正确的合法手机号"));
        } else {
          callback();
        }
      };
      const validatepassword = (rule, value, callback) => {
        if (value.length <= 8 && value.length >= 5) {
          callback();
        } else {
          callback(new Error("请输入正确的密码"));
        }
      };
      const validatenickName = (rule, value, callback) => {
        if (value.length >= 2 && value.length <= 8) {
          callback();
        } else {
          callback(new Error("请输入正确的昵称"));
        }
      };
      return {
        registerForm: {
          // mobile: Cookies.get('loginMobile'),
          mobile: '',
          password: '',
          nickName: '',
          sex: 'MALE'
        },
        registerRules: {
          mobile: [{
            required: true,
            trigger: "blur",
            validator: validateMobile
          }],
          password: [{
            required: true,
            trigger: "blur",
            validator: validatepassword
          }],
          nickName: [{
            required: true,
            trigger: "blur",
            validator: validatenickName
          }]
        },
      }
    },
    methods: {
      handleRegister() {
        this.$refs.registerForm.validate(valid => {
          if (!valid) {
            return false;
          };
          register(this.registerForm)
            .then(res => {
              this.$Message.success("注册成功")
              this.$router.push({ path: "/Login" });
            })
            .catch((err) => {
              this.$Message.error('注册出了点问题', err);
            });
        })
      },
      goLogin(){
          this.$router.push({ path: "/Login" });
      }
    }
  };

</script>

<style>
  .login-container a {
    color: #0078de;
  }

  #canvascontainer {
    position: absolute;
    top: 0px;
  }

  .wz-input-group-prepend {
    background-color: #141a48;
    border: 1px solid #2d8cf0;
    border-right: none;
    color: #2d8cf0;
  }

</style>

<style rel="stylesheet/scss" lang="scss">
  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 5px;
  }

  .login-container {
    height: 120vh;
    margin: 0;
    padding: 0;
    background-color: #2d3a4b;
    background-image: url('../assets/images/bg_login.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    -moz-background-size: cover;
    -webkit-background-size: cover;
    .ivu-input-group-append {
      background-color: #fff;
      color: #4A90E2;
    }
    .code-input {
      input {
        border-right-width: 0px;
      }
    }
    .ivu-input {
      &:hover {
        border-color: #DFDFE6;
      }
      &:focus {
        // border-color: #DFDFE6;
        box-shadow: none;
      }
    }

    input:-webkit-autofill {
      // -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
      // -webkit-text-fill-color: #fff !important;
      -webkit-box-shadow: 0 0 0px 1000px #ddd inset !important;
      -webkit-text-fill-color: #333 !important;
    }
    input {
      background: transparent; // border: 1px solid #2d8cf0;
      // border: 1px solid #fdd000;
      -webkit-appearance: none; // border-radius: 3px;
      padding: 12px 5px 12px 15px; // color: #eeeeee;
      color: #888;
      height: 47px;
    }
    .el-input {
      display: inline-block;
      height: 47px;
      width: 85%;
    }
    .svg-container {
      padding: 6px 5px 6px 15px;
      color: #889aa4;
    }

    .title {
      font-size: 26px;
      font-weight: 400;
      color: #eeeeee;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }

    .login-form {
      position: absolute;
      left: 0;
      right: 0;
      width: 400px;
      background-color: #fff;
      padding: 35px 35px 15px 35px;
      margin: 120px auto;
      overflow-y:visible
    }

    .el-form-item {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }

    .forget-pwd {
      color: #fff;
    }

    .ivu-input-group-append {
      border: 1px solid #dddee1 !important
    }
  }

</style>
