<template>
  <div class="app-container">医院设置表单
    <!-- <el-radio-group v-model="labelPosition" size="small">
  <el-radio-button label="left">左对齐</el-radio-button>
<el-radio-button label="right">右对齐</el-radio-button>
  <el-radio-button label="top">顶部对齐</el-radio-button> -->
<!-- </el-radio-group> --> 
<div style="margin: 16px;"></div>
<el-form :label-position="labelPosition" label-width="100px" :model="searchObj">
  <el-form-item label="医院名称" >
    <el-input v-model="searchObj.hosname" ></el-input>
  </el-form-item>
  <el-form-item label="医院编号" >
    <el-input v-model="searchObj.hoscode"></el-input>
  </el-form-item>
  <el-form-item label="API基础路径">
    <el-input v-model="searchObj.apiUrl"></el-input>
  </el-form-item>
  <el-form-item label="联系人姓名">
    <el-input v-model="searchObj.contactsName"></el-input>
  </el-form-item>
  <el-form-item label="联系人电话" >
    <el-input v-model="searchObj.contactsPhone"></el-input>
  </el-form-item>
</el-form>
<el-button type="primary" @click="saveOrUpdate()" >保存</el-button>
  </div>
</template>


<script>
import hospset from '@/api/yygh/hospset';
  export default {
    data() {
      return {
        labelPosition: 'right',
        searchObj: {
          hosname: '',
          hoscode: '',
          apiUrl: '',
          contactsName:'',
          contactsPhone:''
        }
      }
    },created() {
          },
    methods: { 
      saveOrUpdate() {
         hospset.save(this.searchObj)
        .then(response=>{
          //提示
          this.$message({
            type:"success",
            message:"添加成功"
          })
          //跳转页面  
          this.$router.push({path:'/hospset/list'})
        })
      }
   }
      };
</script>