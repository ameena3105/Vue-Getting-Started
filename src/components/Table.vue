<template>
  <div class="table">
    <el-table
    :data="tableData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
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
          @click="dialogVisible = true"
          
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
    <Edit />
    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">Cancel</el-button>
      <el-button type="primary" @click="dialogVisible = false">Confirm</el-button>
    </span>
  </el-dialog>
  </div>
</template>

<script>
  import Edit from '@/components/Edit.vue'; // @ is an alias to /src
  export default {
    data() {
      return {
        tableData: [{
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
        }],
        search: '',
        dialogVisible: false,
      }
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
          this.tableData.splice(index, 1);
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
      }
    },
    components: {
        Edit,
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
