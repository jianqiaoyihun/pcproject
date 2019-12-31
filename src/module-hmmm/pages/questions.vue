<template>
  <div class="question-container">
    <el-card style="font-size:14px">
      <el-row>
        <el-button>新增试题</el-button>
        <el-button>批量导入</el-button>
      </el-row>
      <el-row style="margin:20px 0;">
        <el-col :span="6">
          <span>学科</span>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="6">
          <span>难度</span>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="6">
          <span>试题类型</span>
          <el-select style="width:150px;margin-left:10px" placeholder="请选择"></el-select>
        </el-col>
        <el-col :span="6">
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
          <el-button size="middle" style="margin-right:20px">清除</el-button>
          <el-button size="middle" type="primary">搜索</el-button>
        </el-col>
      </el-row>
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
        <el-table-column fixed="right" label="操作" width="200">
          <template slot-scope="scope">
            <el-button @click="handleClick(scope.row)" type="text" size="small">预览</el-button>
            <el-button type="text" size="small">修改</el-button>
            <el-button type="text" size="small">删除</el-button>
            <el-button type="text" size="small">加入精选</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-row type="flex" justify="center" style="height:80px" align="middle">
        <el-pagination background layout="prev, pager, next"
        :total="page.total"
        :page-size="page.pageSize"
        :current-page="page.currentPage"
        @current-change="changePage"></el-pagination>
      </el-row>
    </el-card>
  </div>
</template>

<script>
import { list as questionsList } from "../../api/hmmm/questions";
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
    async getQuestionsList() {
      let result = await questionsList();
      this.questionList = result.data.items;
      // console.log(this.questionList)
    }
    
  },
  created() {
    this.getQuestionsList();
  }
};
</script>

<style lang="scss"></style>
