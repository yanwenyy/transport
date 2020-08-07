<template>
  <div class="mod-user">
      <div class="connect-body">
        <div class="connect-list connect-head">
          <div class="inline-block">接口名字</div>
          <div class="inline-block">连接状态<el-button @click="getDataList" size="small" type="primary" style="margin-left: 10px">刷新</el-button></div>
        </div>
        <div class="connect-list">
          <div class="inline-block">用友</div>
          <div :class="dataList.yongyou==200?'green':'red'" class="inline-block connect-status"><i :class="dataList.yongyou==200?'el-icon-success':'el-icon-warning'"></i>{{dataList.yongyou==200?'正常':'异常'}}</div>
        </div>
        <div class="connect-list">
          <div class="inline-block">门禁</div>
          <div :class="dataList.enter==200?'green':'red'" class="inline-block connect-status"><i :class="dataList.enter==200?'el-icon-success':'el-icon-warning'"></i>{{dataList.enter==200?'正常':'异常'}}</div>
        </div>
        <div class="connect-list">
          <div class="inline-block">二维码</div>
          <div :class="dataList.out==200?'green':'red'" class="inline-block connect-status"><i :class="dataList.out==200?'el-icon-success':'el-icon-warning'"></i>{{dataList.out==200?'正常':'异常'}}</div>
        </div>
      </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        dataForm: {

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
    activated () {
      this.getDataList();
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true;
        this.$http({
          url: this.$http.adornUrl('/biz/api/status'),
          method: 'post'
        }).then(({data}) => {
          if (data && data.code === 10000) {
            this.dataList = data.data
            console.log(this.dataList)
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
<style>
  .inline-block{
    display: inline-block;
  }
  .connect-body{
    width: 50%;
    border: 1px solid #999;
    border-radius: 6px;
    font-size: 17px;
  }
  .connect-list{
    display: flex;
    justify-content: space-between;
  }
  .connect-list:not(:last-child){
    border-bottom: 1px solid #999;
  }
  .connect-list>div{
    text-align: center;
    width: 50%;
    padding:10px 0;
  }
  .connect-list>div:first-child{
    border-right: 1px solid #999;
  }
  .connect-head{
    font-size: 18px;
    font-weight: bold;
  }
  .connect-status{
    font-size: 16px;
  }
  .green{
    color: green;
  }
  .red{
    color: red;
  }
</style>
