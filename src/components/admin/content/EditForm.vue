<template>
  <div style="text-align: left">
    <el-button class="add-button" type="success" @click="clickthing">添加外来人员</el-button>
    <el-dialog
      title="添加/修改外来者"
      :visible.sync="dialogFormVisible"
      @close="clear">
      <el-form v-model="form" style="text-align: left" ref="dataForm">
        <el-form-item label="姓名" :label-width="formLabelWidth" prop="name">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="日期" :label-width="formLabelWidth" prop="date">
          <el-input v-model="form.date" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="当前时间" :label-width="formLabelWidth" prop="time">
          <el-input v-model="form.time" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="身份证号" :label-width="formLabelWidth" prop="idCard">
          <el-input v-model="form.idCard" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="体温" :label-width="formLabelWidth" prop="temperature">
          <el-input v-model="form.temperature" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="来访事项" :label-width="formLabelWidth" prop="item">
          <el-input type="textarea" v-model="form.item" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话号码" :label-width="formLabelWidth" prop="phone">
          <el-input v-model="form.phone" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="是否疑似或者确诊" :label-width="formLabelWidth" prop="diagnose">
        <el-select v-model="form.diagnose" placeholder="请选择">
          <el-option label="是" value="是"></el-option>
          <el-option label="否" value="是"></el-option>
        </el-select>
        </el-form-item>
        <el-form-item label="来访人姓名" :label-width="formLabelWidth" prop="accessObject">
          <el-input v-model="form.accessObject" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="具体地址" :label-width="formLabelWidth" prop="address">
        <el-input v-model="form.address" autocomplete="off"></el-input>
      </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="onSubmit">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: 'EditForm',
    data () {
      return {
        date:'',
        time:'',
        dialogFormVisible: false,
        form: {
          name: '',
          date: '',
          time: '',
          idCard: '',
          temperature:'',
          diagnose: '',
          item: '',
          address:'',
          accessObject: '',
          phone:''
        },
        formLabelWidth: '120px'
      }
    },
    created () {
      var aData = new Date();
      this.date = aData.getFullYear() + "-" + (aData.getMonth() + 1) + "-" + aData.getDate();
      var hours=aData.getHours()<10 ? "0"+aData.getHours() : aData.getHours();
      var minutes=aData.getMinutes()<10 ? "0"+aData.getMinutes() : aData.getMinutes();
      var seconds=aData.getSeconds()<10 ? "0"+aData.getSeconds() : aData.getSeconds();
      this.time=hours+":"+minutes+":"+seconds;
    },
    methods: {
      clear () {
        this.form = {
          name: '',
          date: this.date,
          time: this.time,
          idCard: '',
          diagnose: '',
          temperature:'',
          things: '',
          accessObject: '',
          phone:'',
          address:'',
        }
      },
      clickthing(){
        this.dialogFormVisible = true;
        this.form.date=this.date;
        this.form.time=this.time;
      },
      onSubmit () {
        console.log(this.form)
        this.$axios({
          method: 'post',
          url: '/visitorRegister',
          data: JSON.stringify({
            name: this.form.name,
            date: this.form.date,
            time: this.form.time,
            idCard: this.form.idCard,
            diagnose: this.form.diagnose,
            item: this.form.item,
            temperature: this.form.temperature,
            accessObject: this.form.accessObject,
            address: this.form.address,
            phone: this.form.phone,
          }),
          headers: {
            'Content-Type': 'application/json;charset=UTF-8',//设置请求头请求格式为JSON
          },
        }).then(resp => {
            if (resp && resp.data.code === 200) {
              this.dialogFormVisible = false
              this.$emit('onSubmit')
            }
        })
      },
    }
  }
</script>

<style scoped>
  .add-button {
    margin: 18px 0 0 10px;
  }
</style>
