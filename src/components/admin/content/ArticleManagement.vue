<template>
  <div>
    <el-row style="margin: 18px 0px 0px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
        <el-breadcrumb-item>人员管理</el-breadcrumb-item>
        <el-breadcrumb-item>物资管理</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
   <el-card>
   <el-form :model="thingsnum" >
     <el-form-item label="卫生纸" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num1" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="牙膏" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num2" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="洗发液" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num3" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="沐浴露" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num4" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="消毒液" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num5" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="酒精" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num6" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="棉签" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num7" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="999感冒灵" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num8" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="板蓝根" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num9" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="布洛芬胶囊" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num10" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="米" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num11" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="盐" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num12" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="油" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num13" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="萝卜" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num14" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="白菜" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num15" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="辣椒酱" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num16" style="width: 100px"></el-input>
     </el-form-item>
     <el-form-item label="茄子" :label-width="formLabelWidth">
       <el-input readonly v-model="thingsnum.num17" style="width: 100px"></el-input>
     </el-form-item>

   </el-form>
     <el-button @click="seeview">查看今日物资申请</el-button>
   </el-card>

    <el-dialog
      title="今日申请物资"
      style="text-align:left !important"
      :visible.sync="dialog3Visible"
      :before-close="handleClose"
    >
      <el-table
        :data="materalList"
        border
        style="width: 100%"
        stripe
        ref="multipleTable"
        tooltip-effect="dark"
      >
        <el-table-column prop="user" label="身份证号" width="150px"></el-table-column>
        <el-table-column prop="applyDate" label="申请日期" width="150px"></el-table-column>
        <el-table-column prop="category" label="物资分类" width="150px"></el-table-column>
        <el-table-column prop="name" label="物资姓名" width="150px"></el-table-column>
        <el-table-column prop="number" label="物资数量" width="150px"></el-table-column>
      </el-table>
      <el-button type="primary" @click="dialog3Visible = false">取消</el-button>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: 'ArticleManagement',
    data () {
      return {
        thingsnum:{
          num1:1,
          num2:2,
          num3:3,
          num4:4,
          num5:6,
          num6:6,
          num7:6,
          num8:6,
          num9:6,
          num10:6,
          num11:6,
          num12:6,
          num13:6,
          num14:6,
          num15:6,
          num16:6,
          num17:0,
        },
        formLabelWidth: '120px',
        materalList:[],
        dialog3Visible:false,
      }
    },
    mounted () {
      this.getmateralList();
    },
    methods:{
      handleClose(done) {
        done();
      },
      seeview(){
        this.dialog3Visible=true;
      },
      async getmateralList() {
        this.$axios({
          method:'post',
          url:'/materialInfoList',
          headers: {
            'Content-Type': 'application/json;charset=UTF-8',//设置请求头请求格式为JSON
          },
        }).then(res=> {
            console.log(res)
           this.materalList=res.data.result[0]
           this.thingsnum.num1=res.data.result[1][0];
           this.thingsnum.num2=res.data.result[1][1]
          this.thingsnum.num3=res.data.result[1][2]
          this.thingsnum.num4=res.data.result[1][3]
          this.thingsnum.num5=res.data.result[1][4]
          this.thingsnum.num6=res.data.result[1][5]
          this.thingsnum.num7=res.data.result[1][6]
          this.thingsnum.num8=res.data.result[1][7]
          this.thingsnum.num9=res.data.result[1][8]
          this.thingsnum.num10=res.data.result[1][9]
          this.thingsnum.num11=res.data.result[1][10]
          this.thingsnum.num12=res.data.result[1][11]
          this.thingsnum.num13=res.data.result[1][12]
          this.thingsnum.num14=res.data.result[1][13]
          this.thingsnum.num15=res.data.result[1][14]
          this.thingsnum.num16=res.data.result[1][15]
          this.thingsnum.num17=res.data.result[1][16]

          }
        ) .catch(err=>{

        })
      },
    }
  }
</script>

<style scoped>
  .add-link {
    margin: 18px 0 15px 10px;
    float: left;
  }
</style>
