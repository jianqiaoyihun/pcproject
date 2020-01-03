<template>
  <div class="dashboard-container">
    <el-card style="font-size:14px">
      <el-row>
        <el-button @click="$router.push('/questions/new')">新增试题</el-button>
        <el-button>批量导入</el-button>
      </el-row>

          <el-form :model="mydata" ref="myForm" inline label-width="80px" style="margin-top:20px">
            <el-form-item label="学科" prop="subjectValue">
              <el-select style="width:130px;margin-left:10px" placeholder="请选择" v-model="mydata.subjectValue">
                <el-option
                  v-for="item in subjectSimpleLists"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="状态" prop="statusValue">
              <el-select style="width:150px;margin-left:10px" placeholder="请选择" v-model="mydata.statusValue">
                <el-option
                  v-for="item in status"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="难度" prop="difficultyValue">
              <el-select style="width:150px;margin-left:10px" placeholder="请选择" v-model="mydata.difficultyValue">
                <el-option
                  v-for="item in difficulty"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="试题类型" prop="questionTypeValue">
              <el-select style="width:150px;margin-left:10px" placeholder="请选择" v-model="mydata.questionTypeValue">
                <el-option
                  v-for="item in questionType"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="标签" prop="tagsValue">
              <el-input style="width:150px;margin-left:10px" placeholder="请输入" v-model="mydata.tagsValue"></el-input>
            </el-form-item>

            <el-form-item label="录入人" prop="creatorIDValue">
              <el-input style="width:150px;margin-left:10px" placeholder="请输入" v-model="mydata.creatorIDValue"></el-input>
            </el-form-item>

            <el-form-item label="关键字" prop="keywordValue">
              <el-input style="width:180px;margin-left:10px" placeholder="请输入题目编号/题干" v-model="mydata.keywordValue"></el-input>
            </el-form-item>

            <el-form-item label="题目备注" prop="remarksValue">
              <el-input style="width:150px;margin-left:10px" placeholder="请输入" v-model="mydata.remarksValue"></el-input>
            </el-form-item>

            <el-form-item label="二级目录" prop="catalogIDValue">
              <el-input style="width:150px;margin-left:10px" placeholder="请输入二级目录" v-model="mydata.catalogIDValue"></el-input>
            </el-form-item>

            <el-form-item label="城市" prop="provincesValue">
              <el-select @change="changeCity" style="width:150px;margin-left:10px" placeholder="请选择" v-model="mydata.provincesValue">
                <el-option
                  v-for="(item,index) in provincesList"
                  :key="index"
                  :label="item"
                  :value="item"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="区域" prop="citysValue">  
              <el-select style="width:150px;margin-left:10px" placeholder="请选择" v-model="mydata.citysValue">
                <el-option
                  v-for="(item,index) in citysList"
                  :key="index"
                  :label="item"
                  :value="item"
                ></el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="企业简称" prop="shortNameValue">
              <el-input style="width:150px;margin-left:10px" placeholder="请输入题目编号/题干" v-model="mydata.shortNameValue"></el-input>
            </el-form-item>

            <el-form-item label="方向" prop="directionValue">
              <el-input style="width:150px;margin-left:10px" placeholder="请输入" v-model="mydata.directionValue"></el-input>
            </el-form-item>

            <el-form-item>
              <el-button size="small" @click="clearAll">清除</el-button>
              <el-button size="small" type="primary">搜索</el-button>
            </el-form-item>

          </el-form>

   
          <el-table :data="questionList" border style="width: 100%">
            <el-table-column fixed prop="id" label="序号" width="50"></el-table-column>
            <el-table-column prop="number" label="试题编号" width="120"></el-table-column>
            <el-table-column prop="subjectID" label="学科" width="90" :formatter="formatterSubject"></el-table-column>
            <el-table-column prop="questionType" label="题型" width="110" :formatter="formatterType"></el-table-column>
            <el-table-column prop="question" label="题干" width="130"></el-table-column>
            <el-table-column prop="addDate" label="录入时间" width="100">
              <template slot-scope="obj">{{ obj.row.addDate | parseTimeByString }}</template>
            </el-table-column>
            <el-table-column prop="creator" label="录入人" width="100"></el-table-column>
            <el-table-column prop="difficulty" label="难度" width="120" :formatter="formatterDifficulty"></el-table-column>
            <el-table-column prop="zip" label="使用次数" width="60"></el-table-column>
            <el-table-column prop="chkState" label="审核状态" width="80" :formatter="formatterChkState"></el-table-column>
            <el-table-column prop="chkRemarks" label="审核意见" width="80"></el-table-column>
            <el-table-column prop="chkUser" label="审核人" width="70"></el-table-column>
            <el-table-column prop="publishState" label="发布状态" width="80" :formatter="formatterPublishState"></el-table-column>
            <el-table-column fixed="right" label="操作" width="200">
              <template slot-scope="scope">
                <el-button type="text" size="small">预览</el-button>
                <el-button type="text" size="small" @click="$router.push('/questions/new')">修改</el-button>
                <el-button type="text" size="small" @click="handleClick(scope.row)">删除</el-button>
                <el-button type="text" size="small" >加入精选</el-button>
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
    </el-card>
  </div>
</template>

<script>
import { choice as questionsChoies,remove } from "../../api/hmmm/questions";
import { simple as subjectSimpleList } from "../../api/hmmm/subjects";
import { status, difficulty, questionType } from "../../api/hmmm/constants";
import { provinces, citys } from "../../api/hmmm/citys";
export default {
  data() {
    return {
      questionList: [],
      provincesList: [],
      citysList: [],
      mydata: {
        subjectValue: '',
        statusValue: '',
        difficultyValue: '',
        questionTypeValue: '',
        tagsValue: null,
        provincesValue: '',
        citysValue: '',
        keywordValue: null,
        remarksValue: null,
        shortNameValue: null,
        directionValue: null,
        creatorIDValue: null,
        catalogIDValue: null
      },
      status,
      difficulty,
      questionType,
      subjectSimpleLists: [],
      page: {
        total: 8,
        currentPage: 1,
        pageSize: 4
      }
    };
  },
  methods: {
    async getQuestionsChoiceList() {
      let result = await questionsChoies();
      this.questionList = result.data.items;
    },
    async getSubjectSimpleList() {
      let result = await subjectSimpleList();
      this.subjectSimpleLists = result.data;
      // console.log(this.subjectSimpleLists)
      // 获取城市数据
      this.provincesList = await provinces()
    },
    async handleClick(row){
      await this.$confirm("您确定要删除吗")
      await remove(row)
      alert("删除成功") 
      this.getQuestionsChoiceList();
    },
    changeCity(item) {
      console.log(item)
      this.citysList = citys(item)
      // console.log(this.citysList)
    },
    changePage() {
      console.log("别报错了")
    },
    clearAll() {
      this.$refs.myForm.resetFields();
    },
    formatterSubject(row, column, cellValue, index) {
      let result = this.subjectSimpleLists.filter(item => item.value === cellValue); // 得到一个筛选后的数据
      return result.length ? result[0].label : "未知状态";
    },
    formatterType(row, column, cellValue, index) {
      let result = this.questionType.filter(item => item.value == cellValue); // 得到一个筛选后的数据
      return result.length ? result[0].label : "未知状态";
    },
    formatterDifficulty(row, column, cellValue, index) {
      let result = this.difficulty.filter(item => item.value == cellValue); // 得到一个筛选后的数据
      return result.length ? result[0].label : "未知状态";
    },
    formatterChkState(row, column, cellValue, index) {
      let result = this.difficulty.filter(item => item.value == cellValue); // 得到一个筛选后的数据
      return cellValue ? "已审核" : "待审核";
    },
    formatterPublishState(row, column, cellValue, index) {
      let result = this.difficulty.filter(item => item.value == cellValue); // 得到一个筛选后的数据
      return cellValue ? "已发布" : "待发布";
    },
  },
  created() {
    this.getQuestionsChoiceList();
    this.getSubjectSimpleList();
    console.log(this.radio)
  },
  
};
</script>

<style scoped lang="scss"></style>
