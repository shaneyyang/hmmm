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
  <el-form-item label="题型：">
      <el-radio-group v-model="addForm.questionType">
    <el-radio v-for="item in questionTypeList" :label="item.value+''" :key="item.value">{{item.label}}</el-radio>
  </el-radio-group>
  </el-form-item>
  <el-form-item label="难度：">
      <el-radio-group v-model="addForm.difficulty">
    <el-radio v-for="item in difficultyList" :label="item.value+''" :key="item.value">{{item.label}}</el-radio>
  </el-radio-group>
  </el-form-item>
  <el-form-item label="题干：">
  <el-input type="textarea" v-model="addForm.question"></el-input>
</el-form-item>
  <el-form-item label="选项：" v-if="addForm.questionType === '1'">
 <el-radio v-model="selectSingle" :label="0">A.
<el-input v-model="addForm.options[0].title"></el-input>
 </el-radio><br/>
 <el-radio v-model="selectSingle" :label="1">B.
<el-input v-model="addForm.options[1].title"></el-input>
 </el-radio><br/>
 <el-radio v-model="selectSingle" :label="2">C.
<el-input v-model="addForm.options[2].title"></el-input>
 </el-radio><br/>
 <el-radio v-model="selectSingle" :label="3">D.
<el-input v-model="addForm.options[3].title"></el-input>
 </el-radio>

</el-form-item>
  <el-form-item label="选项：" v-if="addForm.questionType === '2'">
    <el-checkbox v-model="addForm.options[0].isRight">A.
      <el-input type="text" v-model="addForm.options[0].title"></el-input>
    </el-checkbox>
    <br>
    <el-checkbox v-model="addForm.options[1].isRight">B.
      <el-input type="text" v-model="addForm.options[1].title"></el-input>
    </el-checkbox>
    <br>
    <el-checkbox v-model="addForm.options[2].isRight">C.
      <el-input type="text" v-model="addForm.options[2].title"></el-input>
    </el-checkbox>
    <br>
    <el-checkbox v-model="addForm.options[3].isRight">D.
      <el-input type="text" v-model="addForm.options[3].title"></el-input>
    </el-checkbox>
</el-form-item>
<template v-if="addForm.questionType==='3'">

</template>

<el-form-item label="答案：">
  <el-input type="textarea" v-model="addForm.answer"></el-input>
</el-form-item>
<el-form-item label="备注：">
  <el-input type="textarea" v-model="addForm.remarks"></el-input>
</el-form-item>
<el-form-item label="标签：">
  <el-input type="text" v-model="addForm.tags"></el-input>
</el-form-item>
<el-form-item>
  <el-button type="primary" @click="addQuestion()">提交</el-button>
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

// 方向列表api // 引入题型api
import {direction as directionList,
questionType as questionTypeList} from '@/api/hmmm/constants'

// 引入城市列表
import {provinces, citys} from '@/api/hmmm/citys'

// 引入企业列表
import { list as companysList } from '@/api/hmmm/companys'

// 导入难度api
import {difficulty as difficultyList} from '@/api/hmmm/constants.js'

// 引入发布api
import { add } from '@/api/hmmm/questions'
export default {
  name: 'QuestionsNew',
  data() {
    return {
      selectSingle: '', // 单选项
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
        options: [
          {code: 'A', title: '', img: '', isRight: false},
          {code: 'B', title: '', img: '', isRight: false},
          {code: 'C', title: '', img: '', isRight: false},
          {code: 'D', title: '', img: '', isRight: false}
        ],
        answer: '', // 答案解析
        remarks: '', // 题目备注
        isPerfect: '', // 是否为精选题
        videoURL: 'http://www.xxx.com' // 解析视频
      },
      subjectList: [], // 学科列表     
      catalogIDList: [], // 二级目录列表   
      enterpriseIDList: [], // 企业
      directionList, // 方向列表
      questionTypeList, // 题型
           
      difficultyList // 难度列表
    }
  },
  watch: {
    // 单选项
    selectSingle(newV) {
      for (var i = 0; i < 4; i++) {
        this.addForm.options[i].isRight = false
      }
      this.addForm.options[newV].isRight = true
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
},
async addQuestion() {
  var result = await add(this.addForm)
  this.$message.success('新增成功')
  this.$router.push('/questions/list')
}
  }
}
</script>

<style scoped>
</style>
