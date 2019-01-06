<template>
  <div class="table">
    <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      label="ID"
      prop="id">
    </el-table-column>
    <el-table-column
      label="Name"
      prop="name">
    </el-table-column>
    <el-table-column
      label="Email"
      prop="email">
    </el-table-column>
    <el-table-column
      align="right">
      <template slot="header">
        <el-input
          v-model="search"
          size="mini"
          placeholder="Type to search"/>
      </template>
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="showPopup(scope.$index, scope.row)"

          icon="el-icon-edit"></el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)"
          icon="el-icon-delete"></el-button>
      </template>
    </el-table-column>
  </el-table>

  <el-dialog
    title="Tips"
    :visible.sync="dialogVisible"
    width="30%"
    :before-close="handleClose">
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
    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">Cancel</el-button>
      <el-button type="primary" @click="updateMethod()">Confirm</el-button>
    </span>
  </el-dialog>
  </div>
</template>

<script>
  import axios from "axios";
  import API from '../config';

  export default {
    data() {
      return {
        /*tableData: [{
          id: '03',
          name: 'Tom',
          email: 'tom@new.com'
        }, {
          id: '02',
          name: 'John',
          email: 'john@new.com'
        }, {
          id: '04',
          name: 'Morgan',
          email: 'morgan@new.com'
        }, {
          id: '01',
          name: 'Jessy',
          email: 'jessy@new.com'
        }],*/
        tableData: [],
        search: '',
        dialogVisible: false,
        dynamicValidateForm: {
          name: '',
          email: '',
          id:'',
        },
      }
    },
    mounted() {
        axios({ method: "GET", "url": API.url + "users/" })
        .then(result => {
            this.tableData= result.data;
            console.log(result);
            //console.log(this.tableData);
        }, error => {
            console.error(error);
        });
    },
    methods: {
      handleClose(done) {
        this.$confirm('Are you sure to close this dialog?')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      handleDelete(index, row) {
        this.$confirm('This will permanently delete the file. Continue?', 'Warning', {
          confirmButtonText: 'OK',
          cancelButtonText: 'Cancel',
          type: 'warning'
        }).then(() => {
          axios({
            method: "POST",
            "url": API.url + "users/delete",
            "data": {
              id: this.tableData[index].id
            }})
          .then(result => {
              this.tableData.splice(index, 1);
              console.log(result);
              //console.log(this.tableData);
          }, error => {
              console.error(error);
          });
          this.$message({
            type: 'success',
            message: 'Delete completed'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: 'Delete canceled'
          });
        });
      },
      showPopup(index, row) {
        this.dialogVisible = true;
        this.dynamicValidateForm.id = row.id;
        console.log(index,row);
      },
      updateMethod() {
        console.log(this.dynamicValidateForm);
        axios({
          method: "POST",
          "url": API.url + "users/update",
          "data": {
            ...this.dynamicValidateForm
          }})
        .then(result => {
            // this.tableData.splice(index, 1);
            console.log(result);
            axios({ method: "GET", "url": API.url + "users/" })
            .then(result => {
              console.log("asdff", result);
                this.tableData= result.data;
                console.log(result);
                this.dialogVisible = false
                //console.log(this.tableData);
            }, error => {
                console.error(error);
            });
            //console.log(this.tableData);
        }, error => {
            console.error(error);
        });

      }
    },

  }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
