<template>
  <a-modal
    :title="title"
    :width="600"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    destroyOnClose
    @cancel="handleCancel"
    cancelText="关闭">

    <a-spin :spinning="confirmLoading">

      <a-form>
        <a-form-item>
          <span>流程名称：</span>
          <a-input
            size="large"
            type="text"
            placeholder="请输入流程名称"
            v-model="name"
          ></a-input>
        </a-form-item>
        <!--<a-form-item>-->
        <!--<span>流程&nbspKEY：</span>-->
        <!--<a-input-->
        <!--size="large"-->
        <!--type="text"-->
        <!--placeholder="流程key是业务流转的关键字，不能重复！"-->
        <!--v-model="key"-->
        <!--&gt;</a-input>-->
        <!--</a-form-item>-->
        <a-form-item>
          <span>流程描述：</span>
          <a-input
            size="large"
            type="text"
            placeholder="流程描述"
            v-model="description"
          ></a-input>
        </a-form-item>

      </a-form>


    </a-spin>
  </a-modal>
</template>

<script>
  import {getAction, httpAction, postAction} from '@/api/manage'
  import DictItemModal from "../../system/modules/DictItemModal";
  import AFormItem from "ant-design-vue/es/form/FormItem";

  export default {
    name: "copyProcActinstModal",
    components: {AFormItem, DictItemModal},
    data() {
      return {
        title: "流程复制【包含按钮、意见、环节、角色全套配置】",
        isClick: false,
        visible: false,
        confirmLoading: false,
        name: undefined,
        key: undefined,
        description: undefined,
        timeRecord: null,
        proMSg: {},
        url: {
          copy: '/workflowSet/copy'
        },
      }
    },
    created() {
    },
    methods: {
      show() {
        this.name = undefined
        this.key = undefined
        this.visible = true
        this.isClick = false
      },
      close() {
        this.$emit('close');
        this.visible = false;
      },
      handleOk() {



        if (this.isClick) {
          this.$message.error("请勿重复点击")
        }
        if (this.name == undefined) {
          this.$message.error('请输入流程名称')
          return
        }
        // if (this.key == undefined) {
        //   this.$message.error('请输入流程key')
        //   return
        // }
        if (this.description == undefined) {
          this.$message.error('请输入描述信息')
          return
        }


        //key校验
        // let keyReg = /^([a-zA-Z])([-_a-zA-Z0-9]{2,19})$/
        // if (!keyReg.test(this.key)) {
        //   if ((this.key).length > 20) {
        //     this.$message.error('您输入的key长度过长')
        //     return
        //   }
        //   if ((this.key).length < 3) {
        //     this.$message.error('您输入的key长度过短')
        //     return
        //   }
        //   this.$message.error('您输入的key不合法请检查')
        //   return
        // }

        if ((this.name).length > 20) {
          this.$message.error('流程名称不能超过20位！')
          return
        }
        if ((this.name).length < 1) {
          this.$message.error('流程名称不能为空！')
          return
        }

        let time = this.timeRecord
        var now = Date.parse(new Date());

        if (time != null) {
          if ((now - time) < 7 * 1000) {
            // let t = (7 - (now - time) / 1000) == 0 ? 1 : (7 - (now - time) / 1000)

            // this.$message.error('您点击过于频繁,请' + (t) + '秒后重试')

            this.$message.error('系统正在处理您的请求,请耐心等待')
            return
          } else {
            // console.log('1111111111111111')
            this.timeRecord = now
          }
        } else {
          // console.log('222222222222222')
          this.timeRecord = now
        }

        // String str1 = java.net.URLDecoder.decode(str1,"utf-8");
        let name = encodeURI(this.name)

        postAction(this.url.copy + '?copyName=' + name + '&sourceDefId=' + this.proMSg.id+'&description='+this.description).then(res => {
          if (res.success) {
            this.isClick = true
            this.$message.success(res.message)
            //刷新父页面
            this.$emit('reload')
            this.handleCancel()

          } else {
            this.$message.error(res.message)
          }
        })

      },
      handleCancel() {
        this.description=''
        this.name=''
        this.close();
      },
    }
  }
</script>

<style lang="less" scoped>
  /deep/ .ant-form-item-children {

    margin-top: 20px;
    display: flex;
    align-items: center;
    justify-content: flex-start;

    span {
      font-size: 16px;
      /*font-weight: bold;*/
      margin-left: 20px;
    }

    input {
      width: 75%;
      margin-left: 10px;

    }
  }

</style>