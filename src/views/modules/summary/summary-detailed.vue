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
      <el-form-item label="排放标准:">
        <el-select clearable  v-model="dataForm.emissionStand" placeholder="请选择">
          <el-option
            v-for="item in pfbz"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button type="warning" @click="down">导出</el-button>
        <!--<el-button v-if="" type="primary" @click="addOrUpdateHandle()">新增</el-button>-->
        <!--<el-button v-if="isAuth('sys:user:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>-->
      </el-form-item>
    </el-form>
    <div class="sumWeigh">总运输量:{{detailList.sumWeigh}}</div>
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
        prop="clientName"
        header-align="center"
        align="center"
        label="供应商">
      </el-table-column>
      <el-table-column
        header-align="center"
        align="center"
        label="运输方式">
        <template slot-scope="scope">
          {{scope.row.tranType==0?'铁路':'公路'}}
        </template>
      </el-table-column>
      <el-table-column
        prop="materialsName"
        header-align="center"
        align="center"
        label="物料名称">
      </el-table-column>
      <el-table-column
        prop="carNum"
        header-align="center"
        align="center"
        label="车牌号">
      </el-table-column>
      <el-table-column
        prop="fuelType"
        header-align="center"
        align="center"
        label="燃油种类">
      </el-table-column>
      <el-table-column
        prop="netWeigh"
        header-align="center"
        align="center"
        label="运输量">
      </el-table-column>
      <el-table-column
        prop="poundRoom"
        header-align="center"
        align="center"
        label="磅房">
      </el-table-column>
      <el-table-column
        prop="enterTime"
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
          materialsNum:'',
          emissionStand:''
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false,
        detailList:'',
        pfbz: [
          {
            value: '国五',
            label: '国五'
          }, {
            value: '国六',
            label: '国六'
          }],
      }
    },
    components: {
    },
    activated () {
      this.dataForm.materialsNum=this.$route.params.list.materialsNum;
      this.detailList=this.$route.params.list;
      this.getDataList();
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true;
        this.$http({
          url: this.$http.adornUrl('/jinding/sum/details'),
          method: 'get',
          params: this.$http.adornParams({
            'pageNum': this.pageIndex,
            'pageSize': this.pageSize,
            'monthTime': this.dataForm.monthTime||'',
            'dayTime': this.dataForm.dayTime||'',
            'materialsNum': this.dataForm.materialsNum
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

      //导出
      down (){
        var url='/jinding/po/sum/details?monthTime='+this.dataForm.monthTime+'&dayTime='+this.dataForm.dayTime+'&tranType=&emissionStand='+this.dataForm.emissionStand+ '&materialsNum='+this.dataForm.materialsNum;
        // console.log(url)
        window.open(this.$http.adornUrl(url));
      },
    }
  }
</script>
<style>
  .sumWeigh{
    font-size: 18px;
    margin-bottom: 10px;
    font-weight: bold;
  }
</style>
