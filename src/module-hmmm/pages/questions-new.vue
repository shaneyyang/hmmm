<template>
  <div class="dashboard-container">
    <div class="app-container">
    <el-card class="box-card">
  <div slot="header" class="clearfix">
    <span>新增试题</span>
  </div>
<el-form ref="addForm" :model="addForm" label-width="180px">
  <el-form-item label="学科：">
    <el-select v-model="addForm.subjectID" placeholder="请选择" clearable>
      <el-option v-for="item in subjectList" :key="item.value" :label="item.label" :value="item.value">
      </el-option>
    </el-select>
  </el-form-item>
  <el-form-item label="目录：">
    <el-select v-model="addForm.catalogID" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in catalogIDList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-form-item>
  <el-form-item label="企业：">
      <el-select v-model="addForm.enterpriseID" clearable>
    <el-option
               v-for="item in enterpriseIDList"
               :key="item.id"
               :label="item.shortName"
               :value="item.id"
            ></el-option>
  </el-select>
  </el-form-item>
  <el-form-item label="城市：">
    <el-select v-model="addForm.province" placeholder="请选择" clearable style="width:90px;" @change="addForm.city=''">
      <el-option v-for="item in provinces()" :key="item" :label="item" :value="item">

      </el-option>
    </el-select>
     <el-select v-model="addForm.city" placeholder="请选择" clearable style="width:90px;">
      <el-option v-for="item in citys(addForm.province)" :key="item" :label="item" :value="item">

      </el-option>
    </el-select>
  </el-form-item>
  <el-form-item label="方向：">
        <el-select v-model="addForm.direction" placeholder="请选择" clearable>
      <el-option v-for="item in directionList" :key="item" :label="item" :value="item">

      </el-option>
    </el-select>
  </el-form-item>
  </el-form>
</el-card>
    </div>
  </div>
</template>

<script>
// 导入api：学科基础简单列表
import {simple} from '@/api/hmmm/subjects.js'

// 二级目录列表
import {simple as directorysSimple} from '@/api/hmmm/directorys'

// 方向列表api
import {direction as directionList} from '@/api/hmmm/constants'

// 引入城市列表
import {provinces, citys} from '@/api/hmmm/citys'

// 引入企业列表
import { list as companysList } from '@/api/hmmm/companys'
export default {
  name: 'QuestionsNew',
  data() {
    return {
      addForm: {
        number: '', // 试题编号
        subjectID: '', // 学科
        catalogID: '', // 目录
        enterpriseID: '', // 企业
        difficulty: '', // 难度
        question: '', // 题干
        questionType: '', // 题型
        tags: '', // 试题标签
        province: '', // 城市
        city: '', // 地区
        direction: '', // 方向
        options: '', // 选项
        answer: '', // 答案解析
        remarks: '', // 题目备注
        isPerfect: '' // 是否为精选题
      },
      subjectList: [], // 学科列表     
      catalogIDList: [], // 二级目录列表   
      enterpriseIDList: [], // 企业
      directionList // 方向列表
    }
  },
  created() {
    // 获取学科简单列表
    this.getSubjectList() 
    // 获取二级目录列表
   this.getCatalogIDList()
   // 获得企业
   this.getEnterpriseIDList() 
  },
  methods: {
    // 获取学科简单列表
    async getSubjectList() {
      let result = await simple()
      this.subjectList = result.data      
    },
  // 由于省份(实际是城市)引入的是一个函数，所以在methods中接收这个函数
  provinces,
  // 接收城市(而实际上应该是县区)
  citys,
    // 获取二级目录列表
  async getCatalogIDList() {
    var result = await directorysSimple()
    this.catalogIDList = result.data
  },
  // 获得 企业 列表信息
async getEnterpriseIDList() {
  var result = await companysList()
  this.enterpriseIDList = result.data.items
}

  }
}
</script>

<style scoped>
</style>
