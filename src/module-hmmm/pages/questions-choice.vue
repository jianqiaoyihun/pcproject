<template>
  <div class="dashboard-container">
    <el-card style="font-size:14px">
      <el-row>
        <el-button>新增试题</el-button>
        <el-button>批量导入</el-button>
      </el-row>
      <el-row style="margin:20px 0;">
        <el-col :span="4">
          <span>学科</span>
          <el-select style="width:130px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="5">
          <span>状态</span>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="5">
          <span>难度</span>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="5">
          <span>试题类型</span>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="5">
          <span>标签</span>
          <el-input style="width:150px;margin-left:10px" placeholder="请输入"></el-input>
        </el-col>
      </el-row>
      <el-row style="margin:20px 0;">
        <el-col :span="6">
          <span>录入人</span>
          <el-input style="width:150px;margin-left:10px" placeholder="请输入"></el-input>
        </el-col>
        <el-col :span="6">
          <span>关键字</span>
          <el-input style="width:180px;margin-left:10px" placeholder="请输入题目编号/题干"></el-input>
        </el-col>
        <el-col :span="6">
          <span>题目备注</span>
          <el-input style="width:150px;margin-left:10px" placeholder="请输入"></el-input>
        </el-col>
        <el-col :span="6">
          <span>二级目录</span>
          <el-input style="width:150px;margin-left:10px" placeholder="请输入二级目录"></el-input>
        </el-col>
      </el-row>
      <el-row style="margin:20px 0;">
        <el-col :span="8">
          <span>城市</span>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="5">
          <span>企业简称</span>
          <el-input style="width:150px;margin-left:10px" placeholder="请输入题目编号/题干"></el-input>
        </el-col>
        <el-col :span="5">
          <span>方向</span>
          <el-input style="width:150px;margin-left:10px" placeholder="请输入"></el-input>
        </el-col>
        <el-col :span="5">
          <el-button size="small">清除</el-button>
          <el-button size="small" type="primary">搜索</el-button>
        </el-col>
      </el-row>
      <el-tabs v-model="activeName" type="card">
        <el-tab-pane label="全部" name="all">
          <el-table :data="questionList" border style="width: 100%">
            <el-table-column fixed prop="id" label="序号" width="50"></el-table-column>
            <el-table-column prop="number" label="试题编号" width="120"></el-table-column>
            <el-table-column prop="subjectID" label="学科" width="90"></el-table-column>
            <el-table-column prop="questionType" label="题型" width="110"></el-table-column>
            <el-table-column prop="question" label="题干" width="130"></el-table-column>
            <el-table-column prop="addDate" label="录入时间" width="120"></el-table-column>
            <el-table-column prop="creator" label="录入人" width="100"></el-table-column>
            <el-table-column prop="difficulty" label="难度" width="120"></el-table-column>
            <el-table-column prop="zip" label="使用次数" width="60"></el-table-column>
            <el-table-column prop="chkState" label="审核状态" width="60"></el-table-column>
            <el-table-column prop="chkRemarks" label="审核意见" width="80"></el-table-column>
            <el-table-column prop="chkUser" label="审核人" width="70"></el-table-column>
            <el-table-column prop="publishState" label="发布状态" width="80"></el-table-column>
            <el-table-column fixed="right" label="操作" width="200">
              <template slot-scope="scope">
                <el-button @click="handleClick(scope.row)" type="text" size="small">审核</el-button>
                <el-button type="text" size="small">预览</el-button>
                <el-button type="text" size="small">下架</el-button>
                <el-button type="text" size="small">修改</el-button>
                <el-button type="text" size="small">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
          <el-row type="flex" justify="center" style="height:80px" align="middle">
            <el-pagination
              background
              layout="prev, pager, next"
              :total="page.total"
              :page-size="page.pageSize"
              :current-page="page.currentPage"
              @current-change="changePage"
            ></el-pagination>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="待审核" name="beforeExamine">配置管理</el-tab-pane>
        <el-tab-pane label="已审核" name="examined">角色管理</el-tab-pane>
      </el-tabs>
    </el-card>
  </div>
</template>

<script>
import { choice as questionsChoies } from "../../api/hmmm/questions";
export default {
  data() {
    return {
      activeName: "all",
      questionList: [],
      questionListPass: [],
      page: {
        total: 8,
        currentPage: 1,
        pageSize: 4
      },
    };
  },
  methods: {
    async getQuestionsChoiceList() {
      let result = await questionsChoies();
      this.questionList = result.data.items;
      // console.log(this.questionList)
    },
    async getQuestionsBeforeExamine() {
      let result = await questionsChoies({ difficulty: "1" });
      this.questionListPass = result.data.items;
      console.log(this.questionListPass);
    }
  },
  created() {
    this.getQuestionsChoiceList();
    this.getQuestionsBeforeExamine();
  }
};
</script>

<style scoped lang="scss">
// 1. 如果在此处 scoped 当前组件下生效样式
// 2. 在style中的所有选择器 编译的时候会自动带上属性选择器
// 3. .box{} ===> .box[data-v-2c9827a4]{} 交集选择器
// 4. 在当前组件下暴露的标签都会加上 data-v-2c9827a4 属性
// 5. 但是如果是组件，其他组件内部的标签是不会加上这个属性 意味写组件内部的样式是不会生效的
// 6. 其他组件的样式都不会生效
</style>
