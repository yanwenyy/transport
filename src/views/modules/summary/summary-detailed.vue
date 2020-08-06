<template>
  <div class="mod-user">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item label="月份:">
        <el-date-picker
          v-model="dataForm.monthTime"
          value-format="yyyy-MM"
          type="month"
          placeholder="选择月份" @change="dataForm.dayTime=''">
        </el-date-picker>
      </el-form-item>
      <el-form-item label="日期:">
        <el-date-picker
          v-model="dataForm.dayTime"
          value-format="yyyy-MM-dd"
          type="date"
          placeholder="选择日期"  @change="dataForm.monthTime=''">
        </el-date-picker>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <!--<el-button v-if="" type="primary" @click="addOrUpdateHandle()">新增</el-button>-->
        <!--<el-button v-if="isAuth('sys:user:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>-->
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      height="80vh"
      border
      v-loading="dataListLoading"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <el-table-column
        type="index"
        header-align="center"
        align="center"
        width="80"
        label="ID">
      </el-table-column>
      <el-table-column
        prop="carWeigh"
        header-align="center"
        align="center"
        label="供应商">
      </el-table-column>
      <el-table-column
        prop="carWeigh"
        header-align="center"
        align="center"
        label="运输方式">
      </el-table-column>
      <el-table-column
        prop="carWeigh"
        header-align="center"
        align="center"
        label="车牌号">
      </el-table-column>
      <el-table-column
        prop="carWeigh"
        header-align="center"
        align="center"
        label="燃油种类">
      </el-table-column>
      <el-table-column
        header-align="center"
        align="center"
        label="运输量">
      </el-table-column>
      <el-table-column
        header-align="center"
        align="center"
        label="磅房">
      </el-table-column>
      <el-table-column
        prop="sumWeigh"
        header-align="center"
        align="center"
        label="进厂时间">
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        dataForm: {
          monthTime: '',
          dayTime: '',
          materialsName:''
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false,
      }
    },
    components: {
    },
    activated () {
      this.getDataList();
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true;
        this.$http({
          url: this.$http.adornUrl('/jinding/sum/list'),
          method: 'get',
          params: this.$http.adornParams({
            'pageNum': this.pageIndex,
            'pageSize': this.pageSize,
            'monthTime': this.dataForm.monthTime||'',
            'dayTime': this.dataForm.dayTime||'',
            'materialsName': this.dataForm.materialsName
          })
        }).then(({data}) => {
          if (data && data.code === 10000) {
            this.dataList = data.data
            this.totalPage = data.total
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },

    }
  }
</script>
