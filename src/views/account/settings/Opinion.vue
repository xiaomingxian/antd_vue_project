<template>
  <a-card :bordered="false">

    <!--&lt;!&ndash; 查询区域 &ndash;&gt;
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">
            <a-col :md="6" :sm="8">
              <a-form-item label="意见内容">
                <a-input placeholder="请输入意见内容" v-model="queryParam.sContent"></a-input>
              </a-form-item>
            </a-col>

          <a-col :md="6" :sm="8" >
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>-->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button  @click="handleAdd" type="primary" icon="plus">新增</a-button>
    <!--  <a-button style="margin-left: 10px" type="primary" icon="download" @click="handleExportXls('快捷意见')">导出</a-button>-->
     <!-- <a-upload style="margin-left: 10px" name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>-->
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="iid"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="pagination"
        :loading="loading"
        @change="handleTableChange"
        :rowClassName="(record,index) => {
              let className  = 'light-row';
              if (index % 2 === 1) className = 'dark-row';
              return className;
          }"
      >

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical" />
          <a @click="handleDelete(record.iid)" title="确定删除吗?">删除</a>
          <!--<a-dropdown>-->
            <!--<a class="ant-dropdown-link">更多 <a-icon type="down" /></a>-->
            <!--<a-menu slot="overlay">-->
              <!--<a-menu-item>-->
                <!--<a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.iid)">-->
                  <!--<a>删除</a>-->
                <!--</a-popconfirm>-->
              <!--</a-menu-item>-->
            <!--</a-menu>-->
          <!--</a-dropdown>-->
        </span>

      </a-table>
      <!--模态框-->
      <a-modal
        title="温馨提示"
        :visible="visibleDel"
        @ok="comfirmDelOrBatchDel"
        @cancel="handleCancel"
      >
        <p>请确认是否要删除该条数据</p>
      </a-modal>
    </div>
    <!-- table区域-end -->
    <!-- 表单区域 -->
    <opinion-modal ref="modalForm" @ok="modalFormOk" ></opinion-modal>
  </a-card>
</template>

<script>
  import OpinionModal from './OpinionModal'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import { getAction,deleteAction } from '@/api/manage'

  export default {
    name: "Opinion",
    mixins:[JeecgListMixin],
    components: {
      OpinionModal
    },
    data () {
      return {
        description: '快捷意见管理页面',
        iisFontSize: '16px',
        visibleDel : false,
        iid:'',
        pagination: {
          current: 1,
          pageSize: 10,
          pageSizeOptions: ['10', '20', '30'],
          showTotal: (total, range) => {
            return range[0] + '-' + range[1] + ' 共' + total + '条'
          },
          showQuickJumper: true,
          showSizeChanger: true,
          total: 0,
          current:''
        },
        // 表头
        columns: [
          {
            title: '序号',
            align:"center",
            width: 60,
            dataIndex: 'iorder'
          },
          {
            title: '意见内容',
            align:"left",
            dataIndex: 'scontent'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            width: 100,
            scopedSlots: { customRender: 'action' },
          }
        ],
        url: {
          list: "/sys_user_opinion/sysUserOpinion/list",
          delete: "/sys_user_opinion/sysUserOpinion/delete",
          deleteBatch: "/sys_user_opinion/sysUserOpinion/deleteBatch",
          exportXlsUrl: "sys_user_opinion/sysUserOpinion/exportXls",
          importExcelUrl: "sys_user_opinion/sysUserOpinion/importExcel",
        },
      }
    },

    created(){

      this.setFontSize();

    },

    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      }
    },
    methods: {
      setFontSize(){
        const  userid =JSON.parse( localStorage.getItem('userdata')).userInfo.id;
        let url = "/testt/sysUserSet/queryByUserId";
        getAction(url,{userId:userid}).then((res) => {
          if(res.result.iisFontSize == 1){
            this.iisFontSize = '18px';
          }else if(res.result.iisFontSize == 3){
            this.iisFontSize = '14px';
          }else{
            this.iisFontSize = '16px';
          }
          document.getElementsByClassName('ant-table')[0].style.fontSize = this.iisFontSize;
        })
      },
      handleDelete(id){
        this.visibleDel = true;
         this.iid = id
      },
      //确认删除模态框
      comfirmDelOrBatchDel(){
          deleteAction(this.url.delete,{id:this.iid}).then((res) => {
            console.log(res.success);
            //--------------删除成功后   刷新列表-----------------
            if(res.success){
              this.$message.success(res.message)
              this.visibleDel = false;
              this.show();
            }

          })
      },
      show(){
        getAction(this.url.list,{pageNo:this.pagination.current,pageSize:10,more:1}).then((res) => {
          this.dataSource = res.result.records;
          this.pagination.total = res.result.total
          alert(res.result.total)

        });
      },
      handleTableChange(pagination){
        console.log(pagination);
        const pager = {...this.pagination};
        pager.current = pagination.current;
        this.pagination = pager;
        getAction(this.url.list,{pageNo:this.pagination.current,pageSize:10,more:1}).then((res) => {
          this.dataSource = res.result.records;
          this.pagination.total = res.result.total

        });

      },
      //确定删除的取消按钮
      handleCancel(){
        this.visibleDel = false;
      },
    }
  }
</script>
<style scoped>
</style>