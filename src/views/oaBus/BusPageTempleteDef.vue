<template>
  <a-modal :visible="visible"
           :title="title"
           :width="900"
           @ok="handleOk"
           @cancel="handleCancel"
           cancelText="关闭">
    <div style="height: 400px; width:100%;overflow: auto">
    <!--定义含义页面，保存所有列-->
     <div style="width: 95%">
    <div class="table-page-search-wrapper" >
      <a-form layout="inline">
        <a-row :gutter="24">
          <a-col :md="6" :sm="8">
            <a-form-item label="字段名：">
              <a-input v-model="stableColumn"></a-input>
            </a-form-item>

          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="初始含义：">
              <a-input  v-model="scolumnName" ></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="字段类型：">
              <a-select placeholder="请选择类型" v-model="icolumnType">
                <a-select-option  v-for="(item,index) in columnType" :key="index" :value="item.value" >{{item.text}}</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" >
              <a-button type="primary" @click="add" icon="search">添加</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>

    <!--<div style="height:60px;width: 100%">
      <h3>
       <div style="display: inline-block;width: 10%;padding-top:10px;">
         <span style="float:right ;line-height:80%;">列名：</span>
       </div>
       <div style="display: inline-block;width: 30%">
         <a-input placeholder="请输入列名" v-model="stableColumn"/>
       </div>
       <div style="display: inline-block;width: 10%">
         <span style="float:right ;line-height:80%;">列的含义：</span>
       </div>
       <div style="display: inline-block;width: 30%">
         <a-input placeholder="请输入列的含义" v-model="scolumnName"/>
       </div>
       <div style="display: inline-block;width: 20%;padding-left:20px;">
         <a-button type="primary" @click="add" icon="plus">添加</a-button>
       </div>
      </h3>
    </div>-->
    <!--<div style="padding-left:20px;" class="table-operator">-->
     <!--<h3>已定</h3>-->
    <!--</div>-->
    <a-table
      ref="table"
      size="middle"
      bordered
      rowKey="id"
      :columns="columns"
      :dataSource="dataSource2"
      :pagination=false
      :loading="loading"
      style="margin-bottom: 30px">
       <span slot="action" slot-scope="text, record">
          <!--<a @click="handleDelect(record.iid)">删除</a>-->
         <a-popconfirm title="确定删除吗?" @confirm="() => handleDelect(record.iid)">
                  <a>删除</a>
          </a-popconfirm>
        </span>
    </a-table>
      </div>
    </div>
  </a-modal>
</template>

<script>
  import {httpAction, getAction,postAction,deleteAction} from '@/api/manage'
  import pick from 'lodash.pick'
  import {JeecgListMixin} from '@/mixins/JeecgListMixin'

  export default {
    name: "BusPageTempleteDef",
    components: {
    },
    data() {
      return {
        title: "配置含义",
        visible: false,
        modelWidth: '100%',
        stableColumn:'',
        scolumnName:'',
        icolumnType:'',
        loading: false,
        columnType:[],
        columns: [
          {
            title: '字段名',
            align: "center",
            width: 220,
            dataIndex: 'stableColumn'
          },
          {
            title: '初始含义',
            align: "center",
            dataIndex: 'scolumnName'
          },
          {
            title: '类型',
            width: 80,
            align: "center",
            dataIndex: 'icolumnType',
            customRender: (text)=> {
              if(undefined == text || text == null){
                return "";
              }
              for(let i = 0 ; i < this.columnType.length ; i ++){
                if(text == this.columnType[i].value){
                  return this.columnType[i].text ;
                }
              }
              return text
            }
          },
          {
            title: '操作',
            dataIndex: 'action',
            align: "center",
            width: 80,
            scopedSlots: {customRender: 'action'},
          }
        ],
        dataSource2: [],
        validatorRules: {
          scolumnName: {rules:[{required:true,message: '列的含义不能为空'},{ min: 1, max: 50, message: '列的含义长度不能超过50位！', trigger: 'blur' }]},
        },
        ipagination: {
          current: 1,
          pageSize: 100,
          //pageSizeOptions: ['10', '20', '30'],
          showTotal: (total, range) => {
            return range[0] + "-" + range[1] + " 共" + total + "条"
          },
          showQuickJumper: true,
          showSizeChanger: true,
          total: 0
        },
        model: {},
        labelCol: {
          xs: {span: 24},
          sm: {span: 5},
        },
        wrapperCol: {
          xs: {span: 24},
          sm: {span: 16},
        },
        confirmLoading: false,
        form: this.$form.createForm(this),
        pageId: '',
        url: {
          add: "/oaBus/busPageDetail/add",
          queryPageDetailDef: "/oaBus/busPageDetail/queryPageDetailDef",
          delete: "/oaBus/busPageDetail/delete",
          columnTypeDict: "/sys/dict/getDictByKeyObj",
        },
      }
    },
    created() {
      this.getSelection();
    },
    methods: {
      //得到字典值,列类型
      getSelection(){
        getAction(this.url.columnTypeDict,{dictKey:"columnType"}).then(res=>{
          this.columnType = res.result ;
          console.log(this.columnType )
        })
      },
      //编辑
      handleDelect(id){
        deleteAction(this.url.delete,{id:id}).then(res=>{
          this.initTable1()
        })
      },
      initTable1(){
        //this.getSelection();
        getAction(this.url.queryPageDetailDef, {pageId: this.pageId}).then((res) => {
          //this.model.busPageDetailList = res.result.records;
          this.dataSource2 = res;
          this.ipagination.total = res.length;
          this.$forceUpdate()
        })
      },
      show(rec) {
        this.visible = true;
        if(rec.spageName != null || rec.spageName != "" || rec.spageName != undefined){
          this.title = rec.spageName ;
        }
        this.pageId = rec.iid ;
        this.initTable1();
      },
      add() {
        if(this.stableColumn.length > 30){
          this.$message.error("字段名长度不能超过30位！");
          return ;
        }
        if(this.scolumnName.length > 30){
          this.$message.error("初始含义长度不能超过30位！");
          return ;
        }
        if(this.stableColumn == undefined || this.stableColumn == ''){
          this.$message.error("请输入字段名！！！");
          return ;
        }
        if(this.scolumnName == undefined || this.scolumnName == ''){
          this.$message.error("请输入初始含义！！！");
          return ;
        }
        if(this.icolumnType == undefined || this.icolumnType == ''){
          this.$message.error("请选择对应的类型！！！");
          return ;
        }
        postAction(this.url.add,{
          ibusPageId:this.pageId,
          stableColumn:this.stableColumn,
          scolumnName:this.scolumnName,
          icolumnType:this.icolumnType,
          iisDefault: 1,
        }).then((res)=>{
          if(res.success){
            this.$message.success(res.message);
            this.initTable1();
          }else{
            this.$message.error(res.message);
          }
        })
      },
      edit(record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model, 'ibusModelId', 'sname', 'ipageId', 'sbusLeftParameter', 'iisUnit', 'iisDept', 'sbusRightParameter', 'iisEditor', 'iisLimits', 'iisEs', 'iisProc', 'iprocSetId', 'screateBy', 'supdateBy'))
        });
      },
      close() {
        //this.$emit('close');
        this.visible = false;
        this.stableColumn = "" ;
        this.scolumnName = "" ;
        this.icolumnType = "" ;
      },
      handleOk() {
        this.close()
      },

      handleCancel() {
        this.close()
      },

    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>
<!--
<style lang="less" scoped>
  .ant-card-body .table-operator{
    margin-bottom: 18px;
  }
  .ant-table-tbody .ant-table-row td{
    padding-top:15px;
    padding-bottom:15px;
  }
  .anty-row-operator button{margin: 0 5px}
  .ant-btn-danger{background-color: #ffffff}

  .ant-modal-cust-warp{height: 100%}
  .ant-modal-cust-warp .ant-modal-body{height:calc(100% - 110px) !important;overflow-y: auto}
  .ant-modal-cust-warp .ant-modal-content{height:90% !important;overflow-y: hidden}
</style>
-->
