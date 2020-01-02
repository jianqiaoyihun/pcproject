<template>
  <el-card>      
        <el-row class="heard">
          <el-button  type="primary" @click="addSub">新增学科</el-button>
            <el-dialog
              :visible.sync="dialogVisible"
              width="30%"
              :before-close="handleClose"
              :show-close='false'>
              <el-form  ref="myForm" :model="formData" :rules="rules" label-width="100px">

              <el-form-item label='学科名称' prop="subjectName" >
                  <el-input v-model="formData.subjectName"></el-input>
              </el-form-item>

              <el-form-item label="是否前台显示" prop="isFrontDisplay">
                <el-switch
                  v-model="formData.isFrontDisplay"
                  active-color="#13ce66"
                  inactive-color="#ff4949"
                  style="padding-left:10px"
                  >
                </el-switch>
              </el-form-item>

             </el-form>
              <span slot="footer" class="dialog-footer"  type='flex' justify="end">
                <el-button @click="beforeClose">取 消</el-button>
                <el-button type="primary" @click="btnOK">确 定</el-button>
              </span>
            </el-dialog>
        </el-row>
  
          <div class="main-top">    
              <el-form inline>
                <el-form-item label='学科名称'>
                    <el-input v-model="subjectName"></el-input>
                </el-form-item>
  
                <el-form-item>
                <el-button class="btn1" @click="clear">清除</el-button>                    
                <el-button class="btn2" @click="search">搜索</el-button>  
                </el-form-item>           
              </el-form>
          </div>
    
          <el-table :data='list' class="sublist">
            <el-table-column  prop="id"  width="100" label="序号"></el-table-column>
            <el-table-column  prop="subjectName" label="学科名称"  width='150'></el-table-column>
            <el-table-column  prop="" label="创建者" width='80'></el-table-column>
            <el-table-column  prop="addDate" label="创建日期">
            <template slot-scope="obj" >
                {{obj.row.addDate | parseTimeByString}}
            </template>
            </el-table-column>
            <el-table-column  prop="isFrontDisplay" label="前台是否显示"  width='120'></el-table-column>
            <el-table-column  prop="twoLevelDirectory" label="二级目录"  width='80'></el-table-column>
            <el-table-column  prop="tags" label="标签"  width='80'></el-table-column>
            <el-table-column  prop="totals" label="题目数量"></el-table-column>
            <el-table-column  label="操作" width="260px">
              <template slot-scope="obj">
                <el-button type='text'>学科分类</el-button>
                <el-button type='text'>学科标签</el-button>
                <el-button type='text'  @click="modify(obj.row.id)">修改</el-button>
                <el-button type='text' @click="delItem(obj.row.id)">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
     
          <el-row  class="subpage" type='flex' justify="end" >
            <el-pagination
            background
            layout="prev, pager, next"
            :current-page="page.currentPage"
            :page-size="page.pageSize"
            :total="page.total"
            @current-change="changePage"
            >
            </el-pagination>
          </el-row>
  </el-card>
 
</template>

<script>
import { list as subjectList,add,remove,detail,update} from '../../api/hmmm/subjects'
export default {
  data() {
    return {
      value: true,
      dialogVisible: false,
      subjectName: "",
      list:[],
      page: {
        // 专门放置分页数据
        total: 0, // 数据总条数
        pageSize: 10, // 默认每页10条
        currentPage: 1, // 当前页码 默认第一页
      },
      formData: {
        subjectName: "",
        isFrontDisplay: true
      },
      rules: {
        subjectName: [{ required: true, message: "学科名称不能为空" }]
      },
    }
  },

  methods:{
    //对话框组件
    handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    },

    //页码改变
      changePage(newPage) {
      this.page.currentPage = newPage;
      this.getCondition();
    },

    getCondition() {
      let params = {
        page: this.page.currentPage,
        pageSize: this.page.pageSize,
        subjectName: this.subjectName
      };
      this.getSubject(params);
    },

    resetFields() {
      this.formData = { subjectName: "", isFrontDisplay: true };
      this.$refs.myForm.clearValidate();
    },
    //修改方法
    async modify(id) {
      let result = await detail({ id });
      this.formData = result.data;
      this.dialogVisible = true;
    },
    //删除方法
    async delItem(id) {
      await this.$confirm("确认删除此目录吗");
      await remove({ id });
      this.getSubject();
    },

    //清空
    clear(){
      this.subjectName=''
    },

    //新增学科
    addSub(){
      this.dialogVisible = true;
    },  

    async btnOK() {
      await this.$refs.myForm.validate();
      this.formData.id ? await update(this.formData) : await add(this.formData);
      this.$message({
        type: "success",
        message: "操作成功"
      });
      this.getCondition();
      this.resetFields();
      this.dialogVisible = false;
    },

    changePage(newPage) {
      this.page.currentPage = newPage;
      this.getCondition();
    },

    //对话框的取消按钮
      beforeClose() {
      this.resetFields();
      this.dialogVisible = false;
    },

     search() {
      this.page.currentPage = 1;
      this.getCondition();
    },

    async getSubject(data) {
      let result = await subjectList(data);
      this.list = result.data.items;
      this.page.total = result.data.counts;
    }
  },
  created() {
    this.getCondition();
  } 
}
</script>

<style  scoped>
  .heard{
    height: 70px;
    border-bottom: 1px solid #ebeef5;
    display: flex;
    align-items: center;
  }
  .main-top{
    padding-top: 20px;
  }
  .btn1{
    margin-left: 10px;
  }
  .btn2{
    background-color:#409EFF;
    color:white  
  }
  .main-body{
    margin-top: 20px;
  }
 .subpage{
   margin-top: 20px;
 }
 .subwitch{
   margin-top: 10px;
 }
  
</style>
