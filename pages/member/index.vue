<template>
  <el-col :span="16" :offset="4">
    <el-row class="header">
      <el-col :span="20"><h1>Member</h1></el-col>
      <el-col :span="4"
        ><el-button type="success" @click="$router.push({ name: 'member-add' })"
          >Add</el-button
        ></el-col
      >
    </el-row>
    <no-ssr>
      <!-- TODO : Add no ssr for not render in Server  -->
      <data-tables :data="users" :total="10">
        <div slot="empty" style="color: red">Users is empty</div>
        <el-table-column prop="username" label="Username" width="200" sortable>
        </el-table-column>
        <el-table-column prop="name" label="Name" width="400" sortable>
        </el-table-column>
        <el-table-column prop="email" label="Email" width="200" sortable>
        </el-table-column>
        <el-table-column fixed="right">
          <template slot-scope="scope">
            <el-button type="text" size="small" @click="editUser(scope.row)">Edit</el-button>
            <el-button type="text" size="small" @click="deleteUser(scope.row.username)">Delete</el-button>
          </template>
        </el-table-column>
      </data-tables>
    </no-ssr>
  </el-col>
</template>

<script>
export default {
  data () {
    return {
      isLoading: false,
    }
  },
  mounted() {
    this.getUserData();
  },
  methods: {
     async getUserData() {
        this.isLoading = true;
        const res = await this.$axios.get("api/users")
        console.log(res.data);
        this.isLoading = false;
    },
    editUser (row) {
      console.log(row)
      this.$router.push({ 
        name: 'member-edit-id', 
        params: {
          id: row.username
        }
      })
    },
     async deleteUser(id) {
      await this.$axios.delete(`api/users/${id}`)
      await this.getUserData()
     },
  },
  head() {
    return {
      title: 'Member System' // tag title ใน head
    }
  },
  async asyncData({ $axios }) {
    const userRequest = await $axios.get('/api/users')
    return { users: userRequest.data.data }
  },
};
</script>
<style scoped>
.header {
  margin-top: 20px;
  margin-bottom: 20px;
}
</style>
