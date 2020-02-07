<template>
  <div class="dashboard-container">
    <div class="app-container">
    <el-card class="box-card">
  <div slot="header" class="clearfix">
    <span>基础题库管理</span>
  </div>
  <el-row>
  <el-col>
    <el-button type="primary" size="mini" @click="$router.push('/questions/new')">
      新增试题
    </el-button>
    <el-button type="danger" size="mini">
      批量导入
    </el-button>
  </el-col>
</el-row>
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
      <el-option v-for="item in tagsList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-col>

</el-row>
  <el-row :gutter="20">
  <el-col :span="6">城市：
    <el-select v-model="searchForm.province" placeholder="请选择" clearable style="width:90px;" @change="searchForm.city=''">
      <el-option v-for="item in provinces()" :key="item" :label="item" :value="item">

      </el-option>
    </el-select>
     <el-select v-model="searchForm.city" placeholder="请选择" clearable style="width:90px;">
      <el-option v-for="item in citys(searchForm.province)" :key="item" :label="item" :value="item">

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
      <el-option v-for="item in directionList" :key="item" :label="item" :value="item">

      </el-option>
    </el-select>
  </el-col>
  <el-col :span="6">录入人：
    <el-select v-model="searchForm.creatorID" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in usersList" :key="item.id" :value="item.id" :label="item.username"></el-option>
    </el-select>
  </el-col>
  <el-col :span="6">二级目录：
    <el-select v-model="searchForm.catalogID" placeholder="请选择" clearable class="wd">
      <el-option v-for="item in catalogIDList" :key="item.value" :label="item.label" :value="item.value">

      </el-option>
    </el-select>
  </el-col>
  <el-col :span="6">
   <el-button>清除</el-button>
  <el-button type="primary">搜索</el-button>
  </el-col>

</el-row>

<el-table :data="questionsList" style="width:100%">
  <el-table-column label="序号" type="index" width="60"></el-table-column>
  <el-table-column label="试题编号" prop="number"></el-table-column>
  <el-table-column label="学科" prop="subject"></el-table-column>
  <el-table-column label="题型" prop="questionType" :formatter="questionTypeFMT"></el-table-column>
  <el-table-column label="题干" prop="question"></el-table-column>
  <el-table-column label="录入时间" prop="addDate" width="170">
    <span slot-scope="stData">{{stData.row.addDate | parseTimeByString}}</span>
  </el-table-column>
  <el-table-column label="难度" prop="difficulty" :formatter="difficultyFMT"></el-table-column>
  <el-table-column label="录入人" prop="creator"></el-table-column>
  <el-table-column label="操作" width="200">
    <template slot-scope="stData">
      <a href="#">预览</a>
      <a href="#">修改</a>
      <!-- 阻止a标签默认的跳转行为，否则点击删除后，弹窗一直在闪 -->
      <a href="#" @click.prevent="del(stData.row)">删除</a>
      <a href="#">加入精选</a>
    </template>

  </el-table-column>
</el-table>

<el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="searchForm.page"
      :page-sizes="[3, 5, 10, 20]"
      :page-size="searchForm.pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="tot">
    </el-pagination>
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

// 导入标签列表api
import {simple as tagsSimple} from '@/api/hmmm/tags'

// 导入录入人api
import {simple as usersSimple} from '@/api/base/users'

// 二级目录列表
import {simple as directorysSimple} from '@/api/hmmm/directorys'
// 方向列表api
import {direction as directionList} from '@/api/hmmm/constants'

// 引入城市列表
import {provinces, citys} from '@/api/hmmm/citys'

// 引入基础题库数据列表  // 引入删除数据api
import {list,
remove
} from '@/api/hmmm/questions'

export default {
  name: 'QuestionsList',
  data() {
    return {
      // 学科列表
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
        catalogID: '',
        page: 1,
        pagesize: 3
     
      },
      tot: 0,
      // 难度列表
      difficultyList,
      // 试题类型列表
      questionList,
      // 标签列表
      tagsList: [],
      // 录入人列表
      usersList: [],
      // 二级目录列表
      catalogIDList: [],
      // 方向列表
      directionList,
      // 县区列表
      cityList: [],
      // 基础题库数据列表
      questionsList: []
    }
  },
  created() {
    // 获取学科简单列表
    this.getSubjectList()
    // 获取标签列表
    this.getTagsList()
    // 获取录入人列表
    this.getUsersList()
    // 获取二级目录列表
   this.getCatalogIDList()
  //  获取基础题库数据列表
  this.getQuestionsList()
  },
  methods: {
    handleSizeChange(val) {
      this.searchForm.pagesize = val
    },
    handleCurrentChange(val) {
      this.searchForm.page = val
    },
    // 删除试题
    del(question) {
  // 确认框
  this.$confirm('确认要删除该记录么?', '删除', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  })
    .then(async () => {
    // 调用remove的api方法，实现删除
    let result = await remove(question)
    // console.log(result)
    this.$message.success('删除成功')
 // 数据刷新(旧的就不显示了)
    this.getQuestionList()
  })
    .catch(() => {})
    },
    // 获取学科简单列表
    async getSubjectList() {
      let result = await simple()
      this.subjectList = result.data      
    },
    // 获取标签列表
  async getTagsList() {
    var result = await tagsSimple()
    this.tagsList = result.data
    
  },
  // 获取录入人列表
  async getUsersList() {
    var result = await usersSimple()
    
    this.usersList = result.data
  },
  // 获取二级目录列表
  async getCatalogIDList() {
    var result = await directorysSimple()
    this.catalogIDList = result.data
  },
  // 由于省份(实际是城市)引入的是一个函数，所以在methods中接收这个函数
  provinces,
  // 接收城市(而实际上应该是县区)
  citys,
  // 接收基础题库数据列表
  async getQuestionsList() {
    var result = await list(this.searchForm)
    this.questionsList = result.data.items
    this.tot = result.data.counts
    // console.log(result)
    
  },
  // 数字转换试题类型
  questionTypeFMT(row, column, cellValue, index) {
    return this.questionList[cellValue - 1].label
  },
  // 数字转换难度
  difficultyFMT(row, column, cellValue) {
  return this.difficultyList[cellValue - 1].label
}

  },
  // 监听数据，搜索后立刻刷新数据
  watch: {
  searchForm: {
    handler: function(newV) {
      this.getQuestionsList()
    },
    // 深度监听
    deep: true
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
.el-table {
 margin-top: 25px;
}
.el-pagination{
  margin-top:15px;
}
</style>
