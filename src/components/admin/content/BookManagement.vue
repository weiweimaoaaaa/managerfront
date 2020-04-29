<template>
  <div>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
        <el-breadcrumb-item>内容管理</el-breadcrumb-item>
        <el-breadcrumb-item>外来人员管理</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <edit-form @onSubmit="loadBooks()" ref="edit"></edit-form>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="books"
        stripe
        style="width: 100%"
        :max-height="tableHeight">
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          fit>
        </el-table-column>
        <el-table-column
          prop="date"
          label="日期"
          sortable
          width="100">
        </el-table-column>
        <el-table-column
          prop="time"
          label="时间"
          fit>
        </el-table-column>
        <el-table-column
          prop="idCard"
          label="身份证号"
          width="120">
        </el-table-column>
        <el-table-column prop="phone" label="电话号码" width="120px"></el-table-column>
        <el-table-column prop="temperature" label="温度" width="120px"></el-table-column>
        <el-table-column prop="diagnose" label="是否疑似或确诊" width="120px">
        </el-table-column>
        <el-table-column prop="item" label="来访事项" width="120px">
        </el-table-column>
        <el-table-column prop="accessObject" label="到访对象" width="120px"></el-table-column>
        <el-table-column prop="address" label="到访人地址" width="120px"></el-table-column>
        <el-table-column
          fixed="right"
          label="操作"
          width="120">
          <template slot-scope="scope">
            <el-button
              @click.native.prevent="editBook(scope.row)"
              type="text"
              size="small">
              编辑
            </el-button>
            <el-button
              @click.native.prevent="deleteBook(scope.row.idcard)"
              type="text"
              size="small">
              移除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <div style="margin: 20px 0 20px 0;float: left">
        <el-button>取消选择</el-button>
        <el-button>批量删除</el-button>
      </div>
    </el-card>
  </div>
</template>

<script>
  import EditForm from './EditForm'
  export default {
    name: 'BookManagement',
    components: {EditForm},
    data () {
      return {
        books: [],
        date:'',
        time:'',
      }
    },
    created () {
      var aData = new Date();
      this.date = aData.getFullYear() + " " + (aData.getMonth() + 1) + " " + aData.getDate();
      var hours=aData.getHours()<10 ? "0"+aData.getHours() : aData.getHours();
      var minutes=aData.getMinutes()<10 ? "0"+aData.getMinutes() : aData.getMinutes();
      var seconds=aData.getSeconds()<10 ? "0"+aData.getSeconds() : aData.getSeconds();
      this.time=hours+":"+minutes+":"+seconds;
    },
    mounted () {
      this.loadBooks()
    },
    computed: {
      tableHeight () {
        return window.innerHeight - 320
      }
    },
    methods: {
      deleteBook (id) {
        this.$confirm('此操作将永久删除该外放人员, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$axios
              .post('/deleteVisitorInfo', {id: id}).then(resp => {
              if (resp && resp.data.code === 200) {
                this.loadBooks()
              }
            })
          }
        ).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          })
        })
      },
      editBook (item) {
        this.$refs.edit.dialogFormVisible = true
        this.$refs.edit.form = {
          name: item.name,
          date: item.date,
          time: item.time,
          idCard: item.idCard,
          diagnose: item.diagnose,
          phone: item.phone,
          temperature:item.temperature,
          item:item.item,
          accessObject:item.accessObject,
          address:item.address,
        }
        // this.$refs.edit.category = {
        //   id: item.category.id.toString()
        // }
      },
      loadBooks () {
        var _this = this
        this.$axios.post('/listVisitorsInfo').then(resp => {
          if (resp && resp.data.code === 200) {
            console.log(resp)
            _this.books = resp.data.result
            console.log(_this.books)
          }
        })
      }
    }
  }
</script>

<style scoped>
</style>
