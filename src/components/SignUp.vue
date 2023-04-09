<template lang="">
  
  <el-card class="box-card">
      <h1>Sign Up</h1>
      <el-form
        ref="ruleFormRef"
        :model="ruleForm"
        label-position="top"
        label-width="150px"
        :rules="rules"
        :size="formSize"
        status-icon
      >
    <el-form-item label="Name" prop="name">
      <el-input v-model="ruleForm.username" />
    </el-form-item>
    <el-form-item label="Email" prop="email">
      <el-input v-model="ruleForm.email" />
    </el-form-item>
    <el-form-item label="Password" prop="password">
      <el-input v-model="ruleForm.password" />
    </el-form-item>
    <el-form-item label="Confirm Password" prop="confirmPassword">
      <el-input v-model="ruleForm.confirmPassword" />
    </el-form-item>
    <el-form-item>
      <el-button type="danger" @click="submitForm(ruleFormRef)">Create Account</el-button>
    </el-form-item>
  </el-form>
    </el-card>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import router from '../router';
import axios from 'axios';

import type { FormInstance, FormRules } from 'element-plus'
const formSize = ref('default')
const ruleFormRef = ref<FormInstance>()

const ruleForm = reactive({
  username: '',
  email: '',
  password: '',
  confirmPassword: '',
})

const rules = reactive<FormRules>({
  username: [
    { required: true, message: 'Please input your name', trigger: 'blur' },
  ],
  email: [
    { required: true, message: 'Please input your email', trigger: 'blur' },
    { type: 'email', message: 'Please input correct email', trigger: 'blur' },
  ],
  password: [
    { required: true, message: 'Please input your password', trigger: 'blur' },
  ],
  confirmPassword: [
    { required: true, message: 'Please confirm your password', trigger: 'blur' },
    {
      validator: (rule, value, callback) => {
        if (value !== ruleForm.password) {
          callback(new Error('Two passwords are inconsistent!'))
        } else {
          callback()
        }
      },
      trigger: 'blur',
    },]
})

const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
      console.log('submit!')
      submitForm_();
    } else {
      console.log('error submit!', fields)
    }
  })
}
const submitForm_ = function(){
  const url = 'http://127.0.0.1:3000/user/register'
  const data = {
    username: ruleForm.username,
    email: ruleForm.email,
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
        router.push('../login')

    })
    .catch(error => {
      
        console.log(error);
      
      
    });
}
</script>

<style scoped>
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
</style>
