<template>
  <a-modal
    :title="title"
    :width="900"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">
      
       <!-- <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="主键id">
          <a-input-number v-decorator="[ 'iId', validatorRules.iid ]" />
        </a-form-item>-->
        <!--<a-form-item-->
          <!--:labelCol="labelCol"-->
          <!--:wrapperCol="wrapperCol"-->
          <!--label="流程按钮关联ID">-->
          <!--<a-input-number v-decorator="[ 'iprocOpinionId', {}]" />-->
        <!--</a-form-item>-->
        <!--<a-form-item-->
          <!--:labelCol="labelCol"-->
          <!--:wrapperCol="wrapperCol"-->
          <!--label="业务流程配置ID">-->
          <!--<a-input-number v-decorator="[ 'iprocSetId', {}]" />-->
        <!--</a-form-item>-->
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="意见框名称">
          <a-input v-decorator="[ 'itaskOpinionName', {}]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="意见框位置(数字)">
          <a-input-number v-decorator="[ 'itaskOpinionOrder', {}]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="流程定义Key">
          <a-input v-decorator="[ 'procDefKey', {}]" />
        </a-form-item>
        <a-form-item
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
          label="任务环节:">
          <a-select
            mode="tags"
            tokenSeparators=","
            style="width: 60%"
            placeholder="请选择任务环节"
            :defaultValue="[]"
            @change="handelChange"
            v-model="taskDefKey">
            <a-select-option v-for="Activity in TaskLink" :value="Activity.id">
              {{ Activity.name }}
            </a-select-option>
          </a-select>
        </a-form-item>


      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>
  import { httpAction,getAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import moment from "moment"

  export default {
    name: "OaOpinionSetModal",
    data () {
      return {
        title:"操作",
        visible: false,
        model: {},
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        data:[],
        taskDefKey:[],
        buttonInfo:'',
        TaskLink:[],//流程定义Id  //任务环节列表
        confirmLoading: false,
        form: this.$form.createForm(this),
        validatorRules:{
        iId:{rules: [{ required: true, message: '请输入主键id!' }]},
        },
        url: {
          add: "/oaopinionset/oaOpinionSet/add",
          edit: "/oaopinionset/oaOpinionSet/edit",
          TaskLink:"/wf/process/actsList"
        },
      }
    },
    created () {
      // this.getButtonList();
    },
    methods: {
      //add () {
        //this.edit({});
      //},
      add (record,TaskLinkId) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        //---------------任务环节---------------
        getAction(this.url.TaskLink,{processDefinitionId:TaskLinkId}).then(res=>{
          this.TaskLink = res.result;
        });

        //分割-数据=数组
        this.taskDefKey=this.model.taskDefKey.split(',');
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'iid','iprocOpinionId', 'iprocSetId',
            'itaskOpinionName','itaskOpinionOrder',
            'taskDefKey','procDefKey'));
		  //时间格式化
        });

      },
      handelChange(value){
        console.log("----------任务环节赋值-------------"+value);
        this.model.taskDefKey=value.toString();
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        this.close()
      },
      handleCancel () {
        this.close()
      }


    }
  }
</script>

<style lang="less" scoped>

</style>