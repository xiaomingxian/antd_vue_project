<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">
          <a-col :md="8" :sm="8" style="margin:0 0 -7px 5px;">
            <a-form-item >
              <a-input placeholder="文件标题" v-model="queryParam.dataTitle"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="8" style="margin:0 0 -7px 5px;">
            <a-form-item >
              <a-input placeholder="文件字号" v-model="queryParam.fileNum"></a-input>
            </a-form-item>
          </a-col>



          <template v-if="toggleSearchStatus">
            <a-col :md="8" :sm="8" style="margin:0 0 -7px 5px;">
              <a-form-item >
                <a-input placeholder="拟稿用户" v-model="queryParam.createName"></a-input>
              </a-form-item>
            </a-col>
            <!--<a-col :md="8" :sm="8">-->

              <!--&lt;!&ndash;style="width: 150px"&ndash;&gt;-->
              <!--<a-form-item label="任务类型">-->
                <!--<a-select  @change="taskTypeChange" v-model="taskType">-->
                  <!--<a-select-option value="全部">全部</a-select-option>-->
                  <!--<a-select-option value="传阅">传阅</a-select-option>-->
                  <!--<a-select-option value="主办">主办</a-select-option>-->
                  <!--<a-select-option value="辅办">辅办</a-select-option>-->
                <!--</a-select>-->
              <!--</a-form-item>-->
            <!--</a-col>-->

            <a-col :md="8" :sm="8" style="margin:0 0 -7px 5px;">
              <a-form-item >
                <a-input placeholder="主办部门" v-model="queryParam.mainDept"></a-input>
              </a-form-item>
            </a-col>

            <!--<a-col :md="8" :sm="8">-->
              <!--<a-form-item >-->
                <!--<a-date-picker style="width: 100%;padding: 0;box-sizing: border-box;" showTime format="YYYY-MM-DD HH:mm:ss" placeholder="开始时间" size="40px"-->
                               <!--v-model='queryParam.startTimeFake'    @change="startTime"/>-->
              <!--</a-form-item>-->
            <!--</a-col>-->
            <!--<a-col :md="8" :sm="8">-->
              <!--<a-form-item >-->
                <!--<a-date-picker showTime format="YYYY-MM-DD HH:mm:ss" placeholder="结束时间" style="width: 100%;padding: 0;box-sizing: border-box;"-->
                               <!--v-model='queryParam.endTimeFake' @change="endTime"/>-->
              <!--</a-form-item>-->
            <!--</a-col>-->


          </template>


          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" ref="reSearchQuery" @click="searchQueryMy" icon="search">查询</a-button>
              <a-button type="primary" @click="searchResetMy" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>

    <!-- 操作按钮区域 -->
    <!--<div class="table-operator">-->
      <!--<a-dropdown v-if="selectedRowKeys.length > 0">-->
        <!--<a-menu slot="overlay">-->
          <!--&lt;!&ndash;<a-menu-item key="1" @click="batchEnd">&ndash;&gt;-->
          <!--&lt;!&ndash;<a-icon type="delete"/>&ndash;&gt;-->
          <!--&lt;!&ndash;批量办结&ndash;&gt;-->
          <!--&lt;!&ndash;</a-menu-item>&ndash;&gt;-->
          <!--<a-menu-item v-if="(url.list=='/wf/task/queryTask?operstatus=my_chuanyue') "-->
                       <!--key="1" @click="batchPiYue">-->
            <!--<a-icon type="delete"/>-->
            <!--批量批阅-->
          <!--</a-menu-item>-->
        <!--</a-menu>-->

        <!--<a-button style="margin-left: 8px"> 批量操作-->
          <!--<a-icon type="down"/>-->
        <!--</a-button>-->
      <!--</a-dropdown>-->

    <!--</div>-->

    <!-- table区域-begin -->
    <div>

      <!--<div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">-->
        <!--<i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{-->
        <!--selectedRowKeys.length }}</a>项-->
        <!--<a style="margin-left: 24px" @click="onClearSelected">清空</a>-->
      <!--</div>-->
<!--        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"-->
      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="processInstanceId"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        @change="handleTableChange"
        :customRow="doTask"
        :rowClassName="(record,index) => {
              let className  = 'light-row';
              if (index % 2 === 1) className = 'dark-row';
              return className;
          }"
      >

        <span slot="action" slot-scope="text, record">

          <a @click.stop="jump(record)">重置流程</a>
          <br>
          <!--<a-divider type="vertical"/>-->
          <!--<a @click.stop="unDo(record)">未办理信息</a>-->
          <!--<br>-->
          <!--<a-divider type="vertical"/>-->
          <a @click.stop="showPic(record)">流转信息</a>

        </span>

      </a-table>
    </div>
    <!-- table区域-end -->

    <!-- 表单区域 -->
    <pic2-modal ref="pic2Modal" @ok="modalFormOk"></pic2-modal>
    <new-task-modal ref="newTask"></new-task-modal>


    <!--业务页面-->
    <detail-file ref="detailFile"></detail-file>
    <back-modal ref="backModal"></back-modal>
    <a-modal
      title="您当前有多个环节的待办，请选择一个环节"
      :width="600"
      :visible="haveMore"
      :confirmLoading="loading"
      @ok="confirm2"
      @cancel="cancel2"
      destroyOnClose
      okText="确认"
      cancelText="取消">


      <!--      :customRow="customRowMy"-->

      <a-table
        ref="table"
        size="small"
        bordered
        rowKey="key"
        :loading="loading"
        :columns="columns3"
        :dataSource="dataSource3"
        :pagination="false"
        :rowSelection="{selectedRowKeys: selectedRowKeys2,selectedRows:selectedRows2, onChange: onSelectChangeMy2,type:'radio'}"
        @change="handleTableChange"
      >
      </a-table>


    </a-modal>

  </a-card>
</template>

<script>
  import Vue from 'vue'
  import {JeecgListMixin} from '@/mixins/JeecgListMixin'
  import {ACCESS_TOKEN} from "../../store/mutation-types";
  import {getAction, postAction} from "../../api/manage";
  import pic2Modal from './modules/pic2Modal'
  //业务页面
  import detailFile from './taskList/detailFile'
  import backModal from './modules/backModal'


  export default {
    name: "taskToDo",
    mixins: [JeecgListMixin],
    components: {
      pic2Modal,
      //业务
      detailFile,
      backModal
    },
    data() {
      return {
        headers: {'X-Access-Token': Vue.ls.get(ACCESS_TOKEN)},
        description: '待办任务',
        iisFontSize: '16px',
        datasource: [],
        // 表头
        columns: [
          {
            title: '序号',
            dataIndex: '',
            key: 'rowIndex',
            width: 60,
            align: "center",
            customRender: function (t, r, index) {
              return parseInt(index) + 1;
            }
          },

          {
            title: '标题',
            sorter: (i, ii, type) => {
              //descend倒叙
              //ascend正序

              this.queryParam.tableOrder = true
              //置空其他环节
              this.nullOther('orederByTile')
              this.queryParam.orederByTile = type == 'descend' ? -1 : 1;
              return true
            },
            align: "left",
            dataIndex: 'title'
          },
          {
            title: '文号',
            width: 120,
            sorter: (i, ii, type) => {

              this.queryParam.tableOrder = true
              this.nullOther('orederByWenHao')

              this.queryParam.orederByWenHao = type == 'descend' ? -1 : 1;
              return true
            },
            align: "center",
            dataIndex: 'wenHao'
          },
          {
            title: '当前环节',
            sorter: (i, ii, type) => {
              this.queryParam.tableOrder = true
              this.nullOther('orederByHuanJie')

              this.queryParam.orederByHuanJie = type == 'descend' ? -1 : 1
              return true
            },
            align: "center",
            width: 110,
            dataIndex: 'name'
          },
          {
            title: '拟稿人',
            sorter: (i, ii, type) => {
              this.queryParam.tableOrder = true
              this.nullOther('orederByDrafter')

              this.queryParam.orederByDrafter = type == 'descend' ? -1 : 1;
              return true
            },
            align: "center",
            width: 100,
            dataIndex: 'drafter'
          },
          {
            title: '转发时间',
            sorter: (i, ii, type) => {
              this.queryParam.tableOrder = true
              this.nullOther('orederByTime')

              this.queryParam.orederByTime = type == 'descend' ? -1 : 1;
              return true
            },
            align: "center",
            width: 180,
            dataIndex: 'createTime'
          },
          {
            title: '审批信息',
            width: 160,
            dataIndex: 'action',
            align: "center",
            scopedSlots: {customRender: 'action'},
          }
        ],
        url: {
          list: "/wf/task/queryTask?operstatus=jump",
          busDataAndColums: 'oaBus/oaBusdata/queryBusdataById',
          yewuDataQuery: "/oaBus/dynamic/query",
          actShowQuery: '/oaBus/busPage/queryActShowByPageRef',
          batchEnd: '/wf/task/batchEnd',
          batchPiYue: '/wf/task/batchPiYue',
          currentUnDo: '/wf/task/queryTaskUnDoCurrent',
          showBackAct: '/wf/task/showBackAct',//展示回退/跳转 节点

        },
        startTimeFake: null,
        endTimeFake: null,
        taskType: null,
        //---------------------------------环节选择相关
        haveMore: false,
        taskRecord: null,
        selectedRowKeys2: [],
        selectedRows2: [],
        dataSource3: [],
        columns3: [
          {
            title: '环节名称',
            align: "center",
            dataIndex: 'name'
          },
          // {
          //   title: 'key',
          //   align: "center",
          //   dataIndex: 'key'
          // }
        ],

      }
    },
    computed: {
      importExcelUrl: function () {
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      }
    }
    ,
    created() {
      //默认不带部门类型
      this.queryParam.isDept = false;
      this.setFontSize();
    }
    ,
    methods: {
      setFontSize() {
        const userid = JSON.parse(localStorage.getItem('userdata')).userInfo.id;
        let url = "/testt/sysUserSet/queryByUserId";
        getAction(url, {userId: userid}).then((res) => {
          if (res.result.iisFontSize == 1) {
            this.iisFontSize = '18px';
          } else if (res.result.iisFontSize == 3) {
            this.iisFontSize = '14px';
          } else {
            this.iisFontSize = '16px';
          }
          document.getElementsByClassName('ant-table')[0].style.fontSize = this.iisFontSize;
        })
      }
      ,
      //清空其他排序条件
      nullOther(type) {
        let orderColums = ['orederByWenHao', 'orederByTile', 'orederByHuanJie', 'orederByDrafter', 'orederByTime']
        for (let i in orderColums) {
          if (i != type) {
            this.queryParam[orderColums[i]] = undefined
          }
        }
      }
      ,
      confirm2() {
        if (this.selectedRowKeys2.length <= 0) {
          this.$message.error("请选择环节")
          return
        }
        this.taskRecord.taskDefinitionKey = this.selectedRowKeys2[0]
        this.taskRecord.name = this.selectedRows2[0].name
        this.taskRecord.id = this.selectedRows2[0].id


        window.open(window.location.origin + '/mytask/taskList/Test-detailFile?tableName=' + this.taskRecord.table + '&busdataId=' + this.taskRecord.tableId + '&status=todo&navisshow=false&haveTask=true&task=' + JSON.stringify(this.taskRecord))
        this.haveMore = false


      },
      cancel2() {
        this.haveMore = false
      },
      onSelectChangeMy2(rowKeys, rows) {
        this.selectedRowKeys2 = rowKeys
        this.selectedRows2 = rows
      },
      jump(record) {
        // var text = type == 'all' ? '跳转' : '回退'
        if (record.id!=undefined && record.id!=null && record.id.indexOf(',')>=0){
          record.id= record.id.split(',')[0]
          record.taskDefinitionKey= record.taskDefinitionKey.split(',')[0]
        }

        getAction(this.url.showBackAct,
          {
            processDefinitionId: record.processDefinitionId,
            type: 'all',
            processInstanceId: record.processInstanceId,
            taskDefinitionKey: record.taskDefinitionKey
          }
        ).then(res => {
          if (res.success) {
            if (res.result.length <= 0) {
              this.$message.error('没有可' + text + '节点')
              return
            }
            this.$refs.backModal.dataSource = res.result
            // this.$refs.backModal.taskMsg = this.taskMsg
            this.$refs.backModal.taskMsg = record
            this.$refs.backModal.title = '流程重置'
            this.$refs.backModal.show()
          } else {
            this.$message.error(res.message)
          }

        })

      }
      ,

      unDo(record) {
        let procInstId = record.processInstanceId
        getAction(this.url.currentUnDo, {
          procInstId: procInstId
        }).then(res => {
          if (res.success) {

            const h = this.$createElement;
            let content = []
            for (let i in res.result) {
              let msg = res.result[i]
              content.push(h('p', `【${msg.taskDefName}】 ${msg.deptName} ：${msg.userName}`))
            }

            let content_ = h('div', {}, content)

            const modal = this.$success({
              title: '未办理信息',
              content: content_,
            });
          } else {
            this.$message.error(res.message)
          }
        })


      }
      ,
      //类型选择
      taskTypeChange(type) {
        if (type != '全部') {
          this.queryParam.isDept = true
          this.queryParam.deptType = type
        } else {
          this.queryParam.isDept = false
        }
      }
      ,
      searchQueryMy() {
        this.queryParam.tableOrder = false

        this.searchQuery()
        this.selectionRows = []
        this.selectedRowKeys = []
      }
      ,
      searchResetMy() {


        this.queryParam.tableOrder = false

        this.searchReset()
        // this.startTimeFake = null
        // this.endTimeFake = null
        this.taskType = null
        this.queryParam.isDept = false
      }
      ,


      endTime(a, time) {
        this.queryParam.endQueryTime = time
      },
      startTime(a, time) {
        this.queryParam.startQueryTime = time
      },
      reload() {
        this.loading = true;
        getAction(this.url.list).then((res) => {
          if (res.success) {
            this.dataSource = res.result.records;
            this.ipagination.total = res.result.total;
          }
          if (res.code === 510) {
            this.$message.warning(res.message)
          }
          this.loading = false;
        })
      }
      ,
      cancel() {
        console.log('------->取消选择')
      }
      ,
      showPic(record) {
        this.$refs.pic2Modal.show(record)
        // this.$refs.pic2Modal.title = '当前环节'
      }
      ,
      //批量办结
      batchEnd() {

        var ids = []
        for (let i in this.selectionRows) {
          if (this.selectionRows[i].processInstanceId != undefined) {
            ids.push(this.selectionRows[i].processInstanceId)
          }
        }
        var param = {ids: ids}

        postAction(this.url.batchEnd, param).then(res => {
          if (res.success) {
            this.$message.success(res.message)
            this.reload()
          } else {
            this.$message.error(res.message)
          }
        })

      }
      ,
      batchPiYue() {
        var data = []
        for (let i in this.selectionRows) {
          if (this.selectionRows[i].id != undefined) {
            let d = this.selectionRows[i]
            let v = {
              table: d.table + '_opinion',
              i_busdata_id: d.tableId,
              s_opinion_type: 4,
              s_task_id: d.id,
              s_task_name: d.name,
              s_overt: 0,
              s_taskdef_key: d.taskDefinitionKey,
            }
            data.push()
          }
        }


        postAction(this.url.batchPiYue, data).then(res => {
          if (res.success) {
            this.$message.success(res.message)
            this.reload()
          } else {
            this.$message.error(res.message)
          }
        })

      }
      ,
      doTask(record, index) {

        return {
          on: {
            click: (event) => {
              this.taskRecord = record


              if (record.id.indexOf(",") >= 0) {
                let keys = record.taskDefinitionKey.split(",")
                let names = record.allNames.split(",")
                let ids = record.id.split(",")
                if (names.length>70){
                  keys=keys.slice(0,69)
                  names=names.slice(0,69)
                  ids=ids.slice(0,69)
                }
                this.dataSource3 = []
                let map = {}

                for (let i in keys) {
                  //构造集合 去重 同一环节去id最大的
                  let data = {key: keys[i], name: names[i], id: ids[i]}
                  let act = map[keys[i]]
                  if (act == undefined) {
                    map[keys[i]] = data
                  } else {
                    if (parseInt(ids[i]) > parseInt(act.id)) {
                      map[keys[i]] = data
                    }
                  }
                }
                //如果去重之后只有一个
                if (Object.values(map).length > 1) {
                  this.dataSource3 = Object.values(map)
                  this.haveMore = true
                }
                if (Object.values(map).length == 1) {

                  let record2 = Object.values(map)[0]
                  this.taskRecord.taskDefinitionKey = record2.key
                  this.taskRecord.name = record2.name
                  this.taskRecord.id = record2.id

                  window.open(window.location.origin + '/mytask/taskList/Test-detailFile?tableName=' + record.table + '&busdataId=' + record.tableId + '&status=todo&navisshow=false&haveTask=true&task=' + JSON.stringify(this.taskRecord))
                }


              } else {
                window.open(window.location.origin + '/mytask/taskList/Test-detailFile?tableName=' + record.table + '&busdataId=' + record.tableId + '&status=todo&navisshow=false&haveTask=true&task=' + JSON.stringify(record))
              }

            }
          }


        }
      },

    }
  }
</script>
<style scoped>
  /*@import '~@assets/less/common.less'*/
  /deep/ .ant-table-middle .ant-table-tbody > tr:hover {
    cursor: pointer;
  }
</style>