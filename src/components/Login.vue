<template>
  <el-card class="box-card">
      <h1>LogIn</h1>
      <el-form
        ref="ruleFormRef"
        :model="ruleForm"
        label-position="top"
        label-width="150px"
        :rules="rules"
        :size="formSize"
        status-icon
      >
    <img class="logo" src="https://attach.setn.com/newsimages/2023/04/05/4107807-PH.jpg"/>
    <el-form-item label="Username" prop="username" >
      <el-input v-model="ruleForm.username" />
    </el-form-item>
    <el-form-item label="Password" prop="password">
      <el-input v-model="ruleForm.password" />
    </el-form-item>


    <el-form-item>
      <el-button type="danger" @click="submitForm_()">LogIn</el-button>
    </el-form-item> 
    <p>{{errorMessege}}</p>
    <div class="signup">
      <p>Don't have account?</p>
      <div class="flex justify-space-between mb-4 flex-wrap gap-4">
        <el-button
          v-for="button in buttons"
          :key="button.text"
          :type="button.type"
          link
          @click="turnToSignPage()"
          >{{ button.text }}
        </el-button>
      </div>
    </div>
  </el-form>
    </el-card>
</template>
<script lang="ts" setup>
import { reactive, ref } from 'vue'
import router from '../router/index.js'
import axios from 'axios';
import type { FormInstance, FormRules } from 'element-plus'
  const formSize = ref('default')
  const ruleFormRef = ref<FormInstance>()
  const ruleForm = reactive({
    username: '',
    password: '',
})
var errorMessege = '';
const rules = reactive<FormRules>({
  username: [
    { required: true, message: 'Please input your email', trigger: 'blur' },
    
  ],
  password: [
    { required: true, message: 'Please input your password', trigger: 'blur' },
  ]
})
const submitForm = async (formEl: FormInstance | undefined) => {
  
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
      console.log('submit!')
      const url = 'http://127.0.0.1:3000/user/login'
      const data = {
        username: ruleForm.username,
        password: ruleForm.password
      }
      
      const config = {
        headers: {
          'Content-Type': 'application/json',
        }
      }
      axios.post(url, data, config)
        .then(response => {
          console.log(response.data);
        })
        .catch(error => {
          console.log(error);
        });
    } else {
      console.log('error submit!', fields)
    }
  })
}
const submitForm_ = function(){
  const url = 'http://127.0.0.1:3000/user/login'
  const data = {
    username: ruleForm.username,
    password: ruleForm.password
  }
  
  const config = {
    headers: {
      'Content-Type': 'application/json',
    }
  }
  axios.post(url, data, config)
    .then(response => {
      
        console.log(response.data);
        router.push('../post')

    })
    .catch(error => {
      
        console.log(error);
        router.push('../ErrorUserName')
      
      
    });
}


const turnToSignPage = function(){
  router.push('/signUp')
};
const buttons = [{ type: 'success', text: 'SignUp' }]
</script>
<style>
.text {
  font-size: 14px;
}
.item {
  padding: 18px 0;
}
.box-card {
  width: 480px;
}
h1 {
  font-size: 24px;
  font-weight: 500;
  margin-bottom: 20px;
  text-align: center;
}
.logo {
  width: 100px; /* 設置元素寬度 */
  height: 100px; /* 設置元素高度 */
  border-radius: 50%; /* 設置邊框半徑為50% */
  margin-left: 170px;
}
.signup{
  display: inline-flex;
  margin-left: auto;
}
</style>