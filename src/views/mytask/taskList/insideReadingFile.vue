<template>
  <center>

    <table border="1" class="sendFileStyle" style="border:#F0F5FC;" width="100%">
      <tr>
        <td class="title" width="15%">
          <!--标题-->
          <center><h3>{{detailList.s_title}}</h3></center>
        </td>
        <td colspan="5">
          <a-textarea maxLength="300" cols="92" rows="2" ref="s_title" v-on:blur="blurText(backData.s_title,$refs.s_title,detailList.s_title)" v-model="backData.s_title"></a-textarea>
        </td>
      </tr>
      <tr>
        <td class="title" width="15%">
          <!--主送单位-->
          <center><h3>{{detailList.s_main_unit_names}}</h3></center>
        </td>
        <td colspan="5">
          <a-textarea maxLength="500" cols="92" rows="2" ref="s_main_unit_names" v-on:blur="blurText(backData.s_main_unit_names,$refs.s_main_unit_names)" v-model="backData.s_main_unit_names"></a-textarea>
        </td>
      </tr>

      <tr>
        <td class="title" width="15%">
          <!--签发人-->
          <center><h3>{{detailList.s_signer}}</h3></center>
        </td>
        <td colspan="3">
          <a-input maxLength="50" style="width:100%;" ref="s_signer" v-on:blur="blurText(backData.s_signer,$refs.s_signer)" v-model="backData.s_signer"></a-input>
        </td>
        <td class="title" width="15%">
          <!--文件字号-->
          <center><h3>{{detailList.s_file_num}}</h3></center>
        </td>
        <td colspan="3">
          <a-input maxLength="32" style="width:100%;" ref="s_file_num" v-on:blur="blurText(backData.s_file_num,$refs.s_file_num)" v-model="backData.s_file_num"></a-input>
        </td>
      </tr>


      <tr style="height: 50px;">
        <td class="title" width="8%">
          <!--类别-->
          <center><h3>{{detailList.s_varchar7}}</h3></center>
        </td>
        <td colspan="3">
          <a-input maxLength="50" style="width:100%;" ref="s_varchar7" v-on:blur="blurText(backData.s_varchar7,$refs.s_varchar7)" v-model="backData.s_varchar7"></a-input>
          <!--<a-textarea cols="92" rows="2" v-model="backData.s_varchar1"></a-textarea>-->
        </td>
        <td class="title" width="8%">
          <!--封发时间-->
          <center><h3>{{detailList.d_sealdate}}</h3></center>
        </td>
        <td colspan="3">
          <a-input  style="width:100%;" ref="d_sealdate" v-on:blur="blurText(backData.d_sealdate,$refs.d_sealdate)" v-model="backData.d_sealdate"></a-input>
          <!--<a-textarea cols="92" rows="2" v-model="backData.s_varchar1"></a-textarea>-->
        </td>
      </tr>
      <tr style="height: 50px;">

        <td class="title" width="15%">
          <!--拟稿人-->
          <center><h3>{{detailList.s_create_name}}</h3></center>
        </td>
        <td colspan="3">
          <div style="padding-left: 10px">{{backData.s_create_name}}</div>
          <!--          <a-input maxLength="50" style="width:100%;" ref="s_varchar5" v-on:blur="blurText(backData.s_varchar5,$refs.s_varchar5)" v-model="backData.s_varchar5"></a-input>-->
        </td>
        <td class="title" width="15%">
          <!--部门-->
          <center><h3>{{detailList.s_create_dept}}</h3></center>
        </td>
        <td colspan="3">
          <div style="padding-left: 10px">{{backData.s_create_dept}}</div>
          <!--          <a-input maxLength="50" style="width:100%;" ref="s_varchar6" v-on:blur="blurText(backData.s_varchar6,$refs.s_varchar6)" v-model="backData.s_varchar6"></a-input>-->
        </td>
      </tr>


      <tr style="height: 50px;">
        <td class="title" width="10%">
          <center><h3>{{detailList.s_varchar3}}</h3></center>
        </td>
        <td colspan="5">
          <div v-for="(item,index) in oaFileList" style="padding-left: 15px">
            <!--<div @click="openFile(9,item.sfileName)"><span class="hoverred">{{index}}、{{item.sfileName}}</span>-->
            <div class="qiCao"><span class="hoverred" @click="qiCao1(9,item)">{{index+1}}、{{item.sfileName}}</span>
              <span class="delCss" v-show="isShowFile">
                <img :title="fileBtnName(1)" v-show ="isSuffex(item.sfileName)" class="pices" @click.stop="qiCao2(10,item)" src="../../../../src/assets/set.png"/>
                <img :title="fileBtnName(2)" class="pices" @click.stop="updateFileName(item)" src="../../../../src/assets/setName.png"/>
                <img :title="fileBtnName(3)" class="pices" @click.stop="deleteFilesBtn(item,4)" src="../../../../src/assets/delete.png"/>
                <img :title="fileBtnName(4)" v-show="oaFileList.length > 1 && index > 0" class="pices" @click.stop="topFile(item,index,4)" src="../../../../src/assets/top.png"/>
                <img :title="fileBtnName(5)" v-show="oaFileList.length > 1 && index < oaFileList.length-1" class="pices" @click.stop="lowFile(item,index,4)" src="../../../../src/assets/bottom.png"/>
              </span>
            </div>
          </div>
          <!--<a-input :value="backData.s_remarks"></a-input>-->
        </td>
      </tr>

      <tr style="height: 50px;">
        <td class="title" width="15%">
          <!--办文依据-->
          <center><h3>{{detailList.s_varchar4}}</h3></center>
        </td>
        <td colspan="5">
          <div v-for="(item,index) in banWenFileList" style="padding-left: 15px">
            <!--<div @click="openFile(9,item.sfileName)"><span class="hoverred">{{index}}、{{item.sfileName}}</span>-->
            <div  class="qiCao"><span class="hoverred" @click="qiCao1(9,item)">{{index+1}}、{{item.sfileName}}</span>
              <span class="delCss" v-show="isShowFile">
                <img :title="fileBtnName(1)" v-show ="isSuffex(item.sfileName)" class="pices" @click.stop="qiCao2(10,item)" src="../../../../src/assets/set.png"/>
                <img :title="fileBtnName(2)" class="pices" @click.stop="updateFileName(item)" src="../../../../src/assets/setName.png"/>
                <img :title="fileBtnName(3)" class="pices" @click.stop="deleteFilesBtn(item,6)" src="../../../../src/assets/delete.png"/>
                <img :title="fileBtnName(4)" v-show="banWenFileList.length > 1 && index > 0" class="pices" @click.stop="topFile(item,index,6)" src="../../../../src/assets/top.png"/>
                <img :title="fileBtnName(5)" v-show="banWenFileList.length > 1 && index < banWenFileList.length-1" class="pices" @click.stop="lowFile(item,index,6)" src="../../../../src/assets/bottom.png"/>
              </span>
            </div>
          </div>
        </td>
      </tr>
    </table>
    <del-time ref="updateFileNameModal" @reloadAfterUpdate="reloadAfterUpdate"></del-time>
  </center>
</template>

<script>
  import {JeecgListMixin} from "../../../mixins/JeecgListMixin2";
  import ATextarea from "ant-design-vue/es/input/TextArea";
  import {busdataTemplate} from "@/views/buttons/btn-js/busdataTemplate";
  import SelDepartName from "../../oaBus/pageModels/selDepartName";
  import DelTime from "../../buttons/DelTimeModal";

  export default {
    name: "insideReadingFile",
    mixins: [JeecgListMixin, busdataTemplate],
    components: {
      DelTime,
      SelDepartName,
      ATextarea,
    },
    props: {
      backDataRef: {
        type: Object,
        required: false
      },
      detailList: {
        type: Object,
        required: false
      },
      optionMap: {
        type: Object,
        required: false
      },
      publishData: {
        type: Object,
        required: false
      },
      isShowFile:{
        type: Object,
        required: false
      }
    },
    data() {
      return {
        s_varchar:[],
        visible: false,
        title: '公文传阅',
        confirmLoading: false,
        userData: '',
        //代办信息
        task: '',
        //已办信息
        taskDone: '',
        oaFileList: [],    //  文档数据   字段含义
        banWenFileList:[],  //办文依据
        i_bigint2_option: [],    //  模块选择
        i_bigint3_option: [],    //  业务选择
        //表中的数据相关值
        backData: {
          //流程key
          key: '',
          act_show: '',
          page_ref: '',
          //业务配置表id-用于查询按钮/意见权限
          iprocSetId: '',
          //对应的业务数据表
          table: '',
          //---------------------------------- 以下为表存储字段
          //*******
          i_id: '',
          i_bigint1: '',
          i_bigint2: '',   //模块
          i_bigint3: '',   //业务类别
          s_receive_type: '',     //来文文种
          d_datetime1: '',//"成文日期"
          //*******
          //模块
          i_bus_model_id: '',
          //
          i_bus_function_id: '',
          //密级
          i_safetylevel: 0,
          //缓急
          i_urgency: 0,
          //主送单位
          s_main_unit_names: '',
          //主办部门
          s_main_dept_names: '主办部门',
          //辅办部门
          s_cc_dept_names: '辅办部门',
          //内部发送部门
          s_inside_deptnames: '内部发送部门',
          //标题
          s_title: '中国人民银行永州市中心支行收文处理签',
          //左侧参数页面头部
          s_left_parameter: '左侧参数页面头部',
          //机构
          s_unit_name: '',
          //部门
          s_dept_name: '',
          //插入参数（修改文头）（页面头部）
          s_middle_parameter: '',
          //右侧参数（页面头部）
          s_right_parameter: '',
          //来文字号
          s_receive_num: '',
          //文件字号
          s_file_num: '',
          //来文机关（数据字典取值）
          i_receive_dept_id: '',
          //是否办结
          i_is_state: 0,
          //是否保存办文单
          i_is_approve: 0,
          //是否归档
          i_is_archives: 0,
          //是否已送全文检索
          i_is_es: 0,
          //是否送公文传输
          i_is_send: 0,
          //是否为临时文件
          i_is_display: 0,
          //备注
          s_remarks: '',
          //创建时间-年
          i_create_year: new Date().getFullYear(),
          //创建时间-月
          i_create_month: new Date().getMonth(),
          //创建时间-日期
          i_create_day: new Date().getDate(),
          //创建人id
          s_create_by: '',
          //创建者姓名
          s_create_name: '',
          //创建人部门name
          s_create_dept: '',
          //创建人部门id
          s_create_deptid: '',
          //创建人机构id
          s_create_unitid: '',
          //签发人
          s_signer: '',
          //封发日期
          d_sealdate: '',
          //创建时间
          d_create_time: '',
          //修改时间
          d_update_time: new Date().getFullYear() + '-' + new Date().getMonth() + '-' + new Date().getDate(),
          s_varchar1: '',
          s_varchar2: '',
          s_varchar3: '',
          s_varchar4: '',
          s_varchar5: '',
          s_varchar6: '',
          s_varchar7: '',
          s_varchar8:'',
          s_varchar9:'',
          s_varchar10:'',
          //当前流程名称
          s_cur_proc_name: '',
          //当前任务名称
          s_cur_task_name: '',
          //是否重要
          i_is_important: ''
        },

        url: {
          optionAndBtn: '/oaBus/newTask/optionAndBtn',
          currentUserMsg: '/sys/user/getUserSectionInfoByToken',
          updateReadStatus: '/oaBus/taskInAct/changeStatus',
          selectTaskType: '/oaBus/busModel/list',
          selectTask: '/oaBus/busModel/list',
          queryOaFileList: '/oaBus/oaFile/queryOaFileList',
          updateBusdata: '/oaBus/dynamic/update'
        }
      }
    },
    created() {
      this.show();
    },

    methods: {
      dateFormat(date) {
        let date1 = new Date(date);
        let Y = date1.getFullYear();
        let M = date1.getMonth() + 1;
        let D = date1.getDate();
        let newTime = Y + '-' + (M < 10 ? "0" + M : M) + "-" + (D < 10 ? "0" + D : D) + " " + date1.toTimeString().substr(0, 8);
        return newTime;
      },
      change(e) {
        this.backData.i_safetylevel = e
      }, changeHuanJi(e) {
        this.backData.i_urgency = e
      },
      //详情页面数据
      showTaskInAct(taskDetail, task) {
        this.task = task
        this.show(taskDetail)
        //修改数据信息为已读
        this.changeReadStatus()
      },
      show(taskDetail) {

        for(let i = 0;i<this.optionMap.checkList.length;i++){
          this.s_varchar.push(this.optionMap.checkList[i].stableColumn);
        }

        //业务数据赋值
        for (var i in this.backData) {
          this.backData[i] = this.backDataRef[i]
        }
        this.getOaFiles(this.backData.table, this.backData.i_id);
        this.getBanWenFiles(this.backData.table, this.backData.i_id);
        this.i_bigint2_option = this.optionMap.i_bigint2_option;
        this.i_bigint3_option = this.optionMap.i_bigint3_option;
      },

      ok() {
        this.handleCancel()
      },

      handleCancel() {
        this.$emit('close');
        this.visible = false;
        this.disableSubmit = false;
        //刷新父页面
        this.$emit('reload_todo')
      }, onChange(date, dateString) {
        // console.log(date, dateString);
      }, onChange2(date, dateString) {
        // console.log(date, dateString);
      },
      close() {
        this.visible = false;
      }

    }
  }
</script>
<style lang="less" scoped>
  @import "../../../assets/less/detailsBaseStyle";

  /*input.ant-input{*/
  /*height: 48px;*/
  /*}*/

  /*textarea.ant-input{*/
  /*height: 48px;*/
  /*}*/

  /*/deep/ .ant-select-selection--single{*/
  /*height: 48px;*/
  /*padding: 8px 0;*/
  /*}*/

  /*/deep/ .ant-calendar-picker-input{*/
  /*height: 48px;*/
  /*}*/
</style>

<style>

  /*.title {*/
  /*background-color: #e9ecf2 !important;*/
  /*padding: 0.7% !important;*/
  /*text-align: center !important;*/
  /*}*/

  /*.text {*/
  /*padding: 10px;*/
  /*}*/


  /*.title h3 {*/
  /*font-weight: bolder !important;*/
  /*}*/

  /*.ant-btn-default {*/
  /*font-weight: bolder !important;*/
  /*background: rgb(178, 218, 255) !important;*/
  /*}*/
</style>