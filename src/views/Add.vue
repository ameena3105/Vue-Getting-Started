<template>
  <div class="add">
    <h1>Add User</h1>
    <el-form :model="dynamicValidateForm" ref="dynamicValidateForm" label-width="120px" class="demo-dynamic">
      <el-form-item
        label="Name"
        prop="name"
        :rules="{
          required: true, message: 'Name can not be null', trigger: 'blur'
        }"
      >
        <el-input v-model="dynamicValidateForm.name"></el-input>
      </el-form-item>
      <el-form-item
        prop="email"
        label="Email"
        :rules="[
          { required: true, message: 'Please input email address', trigger: 'blur' },
          { type: 'email', message: 'Please input correct email address', trigger: ['blur', 'change'] }
        ]"
      >
        <el-input v-model="dynamicValidateForm.email"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('dynamicValidateForm')">Submit</el-button>
        <el-button @click="resetForm()">Reset</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from "axios";
import API from '../config';

  export default {
    data() {
      return {
        dynamicValidateForm: {
          name: '',
          email: ''
        }
      };
    },
    methods: {
      submitForm(formName) {
        axios({
          method: "POST",
          "url": API.url + "users/",
          "data": {
            ...this.dynamicValidateForm
          }})
        .then(result => {
            // this.tableData.splice(index, 1);
            console.log(result);
            this.resetForm();
            //console.log(this.tableData);
        }, error => {
            console.error(error);
        });
      },
      resetForm(formName) {
        this.dynamicValidateForm ={
          name: '',
          email: ''
        }
      },
    }
  }
</script>
