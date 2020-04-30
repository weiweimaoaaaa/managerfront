<template>
  <div>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
        <el-breadcrumb-item>人员管理</el-breadcrumb-item>
        <el-breadcrumb-item>健康信息填报提醒名单</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <el-card style="margin: 18px 2%;width: 95%">
      <el-table
        :data="maninfo"
        stripe
        style="width: 100%">
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column
          prop="id"
          label="身份证号"
          fit>
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          fit>
        </el-table-column>
        <el-table-column
          prop="sex"
          label="性别"
          fit>
        </el-table-column>
        <el-table-column
          prop="phone"
          label="电话号码"
          fit
        >
        </el-table-column>
        <el-table-column
          prop="address"
          label="家庭住址"
          fit
        >
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
    export default {
        name: 'BannerManagement',
        data(){
          return{
            maninfo:[],
          }
        },
      mounted () {
          this.listmaninfo();
      },
      methods:{
          listmaninfo(){
            var _this = this
            this.$axios.post('/getUsersWithoutRegisterHealthInfo').then(resp => {
              if (resp && resp.data.code === 200) {
                console.log(resp)
                _this.maninfo = resp.data.result
              }
            })
          }
      }
    }
</script>

<style scoped>

</style>
