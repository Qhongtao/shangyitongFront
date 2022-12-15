<template>
  <div class="app-container">
        <el-form :inline="true" class="demo-form-inline">
        <el-form-item>
            <el-input
                placeholder="医院名称" v-model="searchObj.hosname" clearable>
            </el-input>
        </el-form-item>
        <el-form-item>
            <el-input placeholder="医院编号" v-model="searchObj.hoscode" clearable>
            </el-input>
        </el-form-item>
            <el-button type="primary" icon="el-icon-search" @click="fetchData()">查询</el-button>
            <!-- <el-button type="default" @click="resetData()">清空</el-button> -->
        </el-form>
    <el-table
    :data="list"
    style="width: 120%"
    :row-class-name="tableRowClassName">
    <el-table-column
      type="index"
      label="序号"
      width="50">
    </el-table-column>
    <el-table-column
      prop="hosname"
      label="医院名称"
      width="180">
    </el-table-column>
    <el-table-column
      prop="hoscode"
      label="医院编号"
      width="180">
    </el-table-column>
    <el-table-column
      prop="apiUrl"
      label="医院API接口">
    </el-table-column>
    <el-table-column
      prop="contactsName"
      label="联系人">
    </el-table-column>
    <el-table-column
      prop="contactsPhone"
      label="电话">
    </el-table-column>
    <el-table-column prop="status" label="状态">
        <template slot-scope="scope">
            {{ scope.row.status===1?'可用':'不可用' }}
        </template>
    </el-table-column>

    <el-table-column label="操作" width="200" align="center">
        <template slot-scope="scope">
            <router-link :to="'/yygh/hospset/edit/'+scope.row.id">
                <el-button type="primary" size="mini" icon="el-icon-edit">修改</el-button>
            </router-link>
            <el-button type="danger" size="mini" icon="el-icon-delete" @click="removeById(scope.row.id)">删除</el-button>
        </template>
    </el-table-column>
    <!-- <el-table-column
      prop="createTime"
      label="创建时间">
    </el-table-column>
    <el-table-column
      prop="updateTime"
      label="更新时间">
    </el-table-column> -->
  </el-table>

    <el-pagination 
    :page-size="20"
    :pager-count="11"
    layout="prev, pager, next"
    :total="total">
    </el-pagination>
  </div>
</template>
<style>
  .el-table .warning-row {
    background: oldlace;
  }

  .el-table .success-row {
    background: #f0f9eb;
  }
  .el-header, .el-footer {
    color: #333;
    text-align: center;
    line-height: 60px;
  }
</style>
<script>
import hospset from '@/api/yygh/hospset'

export default({
    data(){   //定义数据
        return{
            listLoading: true, // 是否显示loading信息
            list: [], // 数据列表
            total: 0, // 总记录数
            page: 1, // 页码
            limit: 10, // 每页记录数
            searchObj: {}// 查询条件
        }
    },
    created(){  // 当页面加载时获取数据
        this.fetchData()
    },
    methods: {
        fetchData(page = 1){ // 调用api层获取数据库中的数据
            console.log('加载列表')
            this.page = page
            this.listLoading = true
            hospset.getPageList(this.page,this.limit,this.searchObj).then(response =>{
                if(response.success == true){
                    this.list = response.data.rows
                    this.total = response.data.total
                }
                this.listLoading = false
            })
        },
        tableRowClassName({row, rowIndex}) {
        if (rowIndex === 1) {
          return 'warning-row';
        } else if (rowIndex === 3) {
          return 'success-row';
        }
        return '';
      },
      removeById(id){
        const h = this.$createElement;
        this.$msgbox({
          title: '注意',
          message: h('p', null, [
            h('span', null, '是否删除 '),
         //   h('i', { style: 'color: teal' }, 'VNode')
          ]),
          showCancelButton: true,
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          beforeClose: (action, instance, done) => {
            if (action === 'confirm') {
              instance.confirmButtonLoading = true,
              hospset.removeById(id)
              instance.confirmButtonLoading = false;
              done();
            } else {
              instance.confirmButtonLoading = false;
              done();
            }
          }
        }).then(action  => {
          this.fetchData()
          this.$message({
            type: 'success',
            message: '删除成功' 
          })
          hospset.getList(1)
        }).catch((action)=>{
            if(action === 'cancel'){
              this.$message({
                type: 'info',
                message: '已取消删除'
              })
            }else{
              this.$message({
                type: 'error',
                message: '删除失败'
            })
            }
          });
      }
      }
  })
  </script>
