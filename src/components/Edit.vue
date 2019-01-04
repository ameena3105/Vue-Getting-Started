<template>
  <div class="edit">
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
    </el-form>
  </div>
</template>

<script>
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

    }
  }
</script>
