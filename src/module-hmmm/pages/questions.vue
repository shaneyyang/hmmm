<template>
  <div class="dashboard-container">
    <div class="app-container">
    <el-card class="box-card">
  <div slot="header" class="clearfix">
    <span>基础题库管理</span>
  </div>
  <el-row :gutter="20">
  <el-col :span="6">学科：
    <el-select v-model="searchForm.subjectID" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in subjectList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-col>
  <el-col :span="6">难&nbsp;&nbsp;&nbsp;&nbsp;度：
    <el-select v-model="searchForm.difficulty" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in difficultyList" :key="item.value" :value="item.value" :label="item.label"></el-option>
    </el-select>
  </el-col>
  <el-col :span="6">试题类型：
    <el-select v-model="searchForm.question" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in questionList" :key="item.value" :value="item.value" :label="item.label"></el-option>
    </el-select>
  </el-col>
  <el-col :span="6">标&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;签：
   <el-select v-model="searchForm.tags" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in subjectList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-col>

</el-row>
  <el-row :gutter="20">
  <el-col :span="6">城市：
    <el-select v-model="searchForm.city" placeholder="请选择" clearable style="width:90px;">
      <el-option v-for="item in subjectList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
     <el-select v-model="searchForm.province" placeholder="请选择" clearable style="width:90px;">
      <el-option v-for="item in subjectList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-col>
  <el-col :span="6">关键字：
    <el-select v-model="searchForm.keyword" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in difficultyList" :key="item.value" :value="item.value" :label="item.label"></el-option>
    </el-select>
  </el-col>
  <el-col :span="6">题目备注：
    <el-input placeholder="请输入" class="wd" v-model="searchForm.remarks"></el-input>
  </el-col>
  <el-col :span="6">企业简称：
   <el-input placeholder="请输入" class="wd" v-model="searchForm.shortName"></el-input>
  </el-col>

</el-row>
  <el-row :gutter="20">
  <el-col :span="6">方向：
    <el-select v-model="searchForm.direction" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in subjectList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-col>
  <el-col :span="6">录入人：
    <el-select v-model="searchForm.creatorID" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in difficultyList" :key="item.value" :value="item.value" :label="item.label"></el-option>
    </el-select>
  </el-col>
  <el-col :span="6">二级目录：
    <el-select v-model="searchForm.catalogID" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in subjectList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-col>
  <el-col :span="6">11：
   
  </el-col>

</el-row>
</el-card>
    </div>

  </div>
</template>

<script>
// 导入api：学科基础简单列表
import {simple} from '@/api/hmmm/subjects.js'

// 导入难度和试题类型api
import {difficulty as difficultyList,
questionType as questionList} from '@/api/hmmm/constants.js'

export default {
  name: 'QuestionsList',
  data() {
    return {
      subjectList: [],
      searchForm: {
        subjectID: '',
        difficulty: '',
        question: '',
        tags: '',
        province: '',
        city: '',
        keyword: '',
        remarks: '',
        shortName: '',
        direction: '',
        creatorID: '',
        catalogID: ''

      },
      difficultyList,
      questionList
    }
  },
  created() {
    // 获取学科简单列表
    this.getSubjectList()
  },
  methods: {
    // 获取学科简单列表
    async getSubjectList() {
      let result = await simple()

      this.subjectList = result.data
      
    }
  }
}
</script>

<style scoped>
.wd{
  width: 170px;
}
.el-row{
  margin-bottom: 10px;
}
</style>
