<template>
  <div class="main user-layout-register">
    <h3><span>用户注册</span></h3>
    <a-form ref="formRegister" :autoFormCreate="(form)=>{this.form = form}" id="formRegister">

      <a-form-item
        fieldDecoratorId="s_create_name"
        :fieldDecoratorOptions="{rules: [{ required: true, message: '申请人不能为空'}, { validator: this.checkUsername }], validateTrigger: ['change', 'blur']}">
        <a-input size="large" type="text" autocomplete="false" placeholder="请输入申请人名称"></a-input>
      </a-form-item>

      <a-form-item
        fieldDecoratorId="s_dept_name"
        :fieldDecoratorOptions="{rules: [{ required: true, message: '申请人部门不能为空'}, { validator: this.nullValueCheck }], validateTrigger: ['change', 'blur']}">
        <a-input size="large" type="text" autocomplete="false" placeholder="请输入申请人部门"></a-input>
      </a-form-item>

      <!--<a-popover placement="rightTop" trigger="click" :visible="state.passwordLevelChecked">-->
      <!--<template slot="content">-->
      <!--<div :style="{ width: '240px' }">-->
      <!--<div :class="['user-register', passwordLevelClass]">强度：<span>{{ passwordLevelName }}</span></div>-->
      <!--<a-progress :percent="state.percent" :showInfo="false" :strokeColor=" passwordLevelColor "/>-->
      <!--<div style="margin-top: 10px;">-->
      <!--<span>请至少输入 8 个字符。请不要使用容易被猜到的密码。</span>-->
      <!--</div>-->
      <!--</div>-->
      <!--</template>-->
      <!--<a-form-item-->
      <!--fieldDecoratorId="password"-->
      <!--:fieldDecoratorOptions="{rules: [{ required: false}, { validator: this.handlePasswordLevel }], validateTrigger: ['change', 'blur']}">-->
      <!--<a-input size="large" type="password" @click="handlePasswordInputClick" autocomplete="false" placeholder="至少8位密码，区分大小写"></a-input>-->
      <!--</a-form-item>-->
      <!--</a-popover>-->

      <!--<a-form-item-->
      <!--fieldDecoratorId="password2"-->
      <!--:fieldDecoratorOptions="{rules: [{ required: true, message: '至少8位密码，区分大小写' }, { validator: this.handlePasswordCheck }], validateTrigger: ['change', 'blur']}">-->

      <!--<a-input size="large" type="password" autocomplete="false" placeholder="确认密码"></a-input>-->
      <!--</a-form-item>-->
      <!--<a-form-item-->
      <!--fieldDecoratorId="email"-->
      <!--:fieldDecoratorOptions="{rules: [{ required: true, type: 'email', message: '请输入正确的邮箱地址' }, { validator: this.handleEmailCheck }], validateTrigger: ['change', 'blur']}">-->
      <!--<a-input size="large" type="text" placeholder="邮箱"></a-input>-->
      <!--</a-form-item>-->
      <a-form-item
        fieldDecoratorId="i_phone"
        :fieldDecoratorOptions="{rules: [{ required: true, pattern: /^1[3456789]\d{9}$/, message: '请输入正确的手机号' }, { validator: this.handlePhoneCheck } ], validateTrigger: ['change', 'blur'] }">
        <a-input size="large" placeholder="11 位手机号">
          <a-select slot="addonBefore" size="large" defaultValue="+86">
            <a-select-option value="+86">+86</a-select-option>
            <a-select-option value="+87">+87</a-select-option>
          </a-select>
        </a-input>
      </a-form-item>

      <a-form-item
        fieldDecoratorId="s_varchar9"
        :fieldDecoratorOptions="{rules: [{ required: true, message: '申请理由不能为空'}, { validator: this.nullValueCheck }], validateTrigger: ['change', 'blur']}">
        <a-input size="large" type="text" autocomplete="false" placeholder="请输入申请理由"></a-input>
      </a-form-item>

      <a-form-item
        fieldDecoratorId="d_create_time">
        <a-input :value="now" disabled size="large" type="text" autocomplete="false" :defaultValue="now"
                 placeholder="申请时间"></a-input>

        <!--<a-input size="large" type="text" autocomplete="false" placeholder="请输入申请理由"></a-input>-->
      </a-form-item>


      <a-form-item
        fieldDecoratorId="s_title"
        :fieldDecoratorOptions="{rules: [{ required: true, message: '机构名称不能为空'}, { validator: this.nullValueCheck }], validateTrigger: ['change', 'blur']}">
        <a-input size="large" type="text" autocomplete="false" placeholder="请输入机构名称"></a-input>
      </a-form-item>


      <a-form-item
        fieldDecoratorId="s_varchar3"
        :fieldDecoratorOptions="{rules: [{ required: true, message: '机构代码不能为空'}, { validator: this.nullValueCheck }], validateTrigger: ['change', 'blur']}">
        <a-input size="large" type="text" autocomplete="false" placeholder="请输入机构代码"></a-input>
      </a-form-item>


      <a-form-item
      fieldDecoratorId="i_bigint1">
      <!--:fieldDecoratorOptions="{rules: [{ required: true, message: '机构类型英文码不能为空'}, { validator: this.nullValueCheckEn }], validateTrigger: ['change', 'blur']}">-->
      <!--<a-input size="large" type="text" autocomplete="false" placeholder="请输入机构类型英文码"></a-input>-->

        <a-input :value="id" disabled size="large" type="text" autocomplete="false" :defaultValue="id"
                 placeholder="申请时间"></a-input>
      </a-form-item>

      <a-form-item
        fieldDecoratorId="s_varchar1"
        :fieldDecoratorOptions="{rules: [{ required: true, message: '机构类型不能为空'}, { validator: this.nullValueCheck }], validateTrigger: ['change', 'blur']}">
        <a-input size="large" type="text" autocomplete="false" placeholder="请输入机构类型"></a-input>
      </a-form-item>


      <!--<a-input-group size="large" compact>
            <a-select style="width: 20%" size="large" defaultValue="+86">
              <a-select-option value="+86">+86</a-select-option>
              <a-select-option value="+87">+87</a-select-option>
            </a-select>
            <a-input style="width: 80%" size="large" placeholder="11 位手机号"></a-input>
          </a-input-group>-->

      <!--<a-row :gutter="16">-->
      <!--<a-col class="gutter-row" :span="16">-->
      <!--<a-form-item-->
      <!--fieldDecoratorId="captcha"-->
      <!--:fieldDecoratorOptions="{rules: [{ required: true, message: '请输入验证码' }], validateTrigger: 'blur'}">-->
      <!--<a-input size="large" type="text" placeholder="验证码">-->
      <!--<a-icon slot="prefix" type="mail" :style="{ color: 'rgba(0,0,0,.25)' }"/>-->
      <!--</a-input>-->
      <!--</a-form-item>-->
      <!--</a-col>-->
      <!--<a-col class="gutter-row" :span="8">-->
      <!--<a-button-->
      <!--class="getCaptcha"-->
      <!--size="large"-->
      <!--:disabled="state.smsSendBtn"-->
      <!--@click.stop.prevent="getCaptcha"-->
      <!--v-text="!state.smsSendBtn && '获取验证码'||(state.time+' s')"></a-button>-->
      <!--</a-col>-->
      <!--</a-row>-->

      <a-form-item>
        <a-button
          size="large"
          type="primary"
          htmlType="submit"
          class="register-button"
          :loading="registerBtn"
          @click.stop.prevent="handleSubmit"
          :disabled="registerBtn">注册
        </a-button>
        <!-- @click="searchResetMy"  margin-left: 20px  -->
        <a-button type="primary" size="large" icon="reload" class="reload-button" style="float:right;">重置</a-button>
        <!--<router-link class="login" :to="{ name: 'login' }">重置</router-link>-->
      </a-form-item>

    </a-form>
    <register-result ref="registerResult" v-show="false"></register-result>
  </div>
</template>

<script>
  import {mixinDevice} from '@/utils/mixin.js'
  import {getSmsCaptcha} from '@/api/login'
  import {getAction, postAction} from '@/api/manage'
  import {checkOnlyUser} from '@/api/api'
  import RegisterResult from './RegisterResult'

  const levelNames = {
    0: '低',
    1: '低',
    2: '中',
    3: '强'
  }
  const levelClass = {
    0: 'error',
    1: 'error',
    2: 'warning',
    3: 'success'
  }
  const levelColor = {
    0: '#ff0000',
    1: '#ff0000',
    2: '#ff7e05',
    3: '#52c41a',
  }
  export default {
    name: "Register",
    components: {RegisterResult},
    mixins: [mixinDevice],
    data() {
      return {
        form: null,
        now: new Date().getFullYear() + '-' + (new Date().getMonth() + 1) + "-" + new Date().getDate()
          + " " + new Date().getHours() + ":" + new Date().getMinutes() + ":" + new Date().getSeconds(),

        table: null,
        id: null,
        state: {
          time: 60,
          smsSendBtn: false,
          passwordLevel: 0,
          passwordLevelChecked: false,
          percent: 10,
          progressColor: '#FF0000'
        },
        registerBtn: false
      }
    },
    computed: {
      passwordLevelClass() {
        return levelClass[this.state.passwordLevel]
      },
      passwordLevelName() {
        return levelNames[this.state.passwordLevel]
      },
      passwordLevelColor() {
        return levelColor[this.state.passwordLevel]
      }
    },
    created() {
      let param = this.$route.query;
      this.table = param.tableName
      this.id = param.busdataId
    },
    methods: {
      // nullValueCheckEn(rule, value, callback){
      //   var re=/^[a-zA-Z]+$/;
      //   if(!re.test(value)){
      //     callback('请输入英文机构码')
      //   }
      //   callback()
      // },
      nullValueCheck(rule, value, callback) {
        if (value == undefined || value == null || value == '') {
          callback()
        }
        callback()
      },
      checkUsername(rule, value, callback) {
        var params = {
          username: value,
        };
        checkOnlyUser(params).then((res) => {
          if (res.success) {
            callback()
          } else {
            callback("用户名已存在!")
          }
        })
      },
      handleEmailCheck(rule, value, callback) {
        var params = {
          email: value,
        };
        checkOnlyUser(params).then((res) => {
          if (res.success) {
            callback()
          } else {
            callback("邮箱已存在!")
          }
        })
      },
      handlePasswordLevel(rule, value, callback) {

        let level = 0;
        //let reg = /^(?=.*[a-zA-Z])(?=.*\d)(?=.*[~!@#$%^&*()_+`\-={}:";'<>?,./]).{8,}$/;
        let reg = /^(?=.*[a-zA-Z~!@#$%^&*()_+`\-={}:";'<>?,./\d]).{6,}$/;
        if (!reg.test(value)) {
          //callback(new Error('密码由8位数字、大小写字母和特殊符号组成!'))
          callback(new Error('密码由至少6位字符组成!'))
        }
        // 判断这个字符串中有没有数字
        if (/[0-9]/.test(value)) {
          level++
        }
        // 判断字符串中有没有字母
        if (/[a-zA-Z]/.test(value)) {
          level++
        }
        // 判断字符串中有没有特殊符号
        if (/[^0-9a-zA-Z_]/.test(value)) {
          level++
        }
        this.state.passwordLevel = level
        this.state.percent = level * 30
        if (level >= 2) {
          if (level >= 3) {
            this.state.percent = 100
          }
          callback()
        } else {
          if (level === 0) {
            this.state.percent = 10
          }
          callback(new Error('密码强度不够'))
        }
      },

      handlePasswordCheck(rule, value, callback) {
        let password = this.form.getFieldValue('password')
        //console.log('value', value)
        if (value === undefined) {
          callback(new Error('请输入密码'))
        }
        if (value && password && value.trim() !== password.trim()) {
          callback(new Error('两次密码不一致'))
        }
        callback()
      },

      handlePhoneCheck(rule, value, callback) {
        var params = {
          phone: value,
        };
        checkOnlyUser(params).then((res) => {
          if (res.success) {
            callback()
          } else {
            callback("手机号已存在!")
          }
        })
      },

      handlePasswordInputClick() {
        if (!this.isMobile()) {
          this.state.passwordLevelChecked = true
          return;
        }
        this.state.passwordLevelChecked = false
      },

      handleSubmit() {
        this.form.validateFields((err, values) => {

          if (!err) {
            values.table=this.table
            values.i_id=this.id
            postAction("/registerNoShiro/updata", values).then((res) => {
              if (!res.success) {
                this.registerFailed(res.message)
              } else {
                values.username=values.s_create_name
                this.$router.push({name: 'registerResult', params: {...values}})
              }
            })
          }
        })
      },

      getCaptcha(e) {
        e.preventDefault()
        let that = this
        this.form.validateFields(['mobile'], {force: true}, (err, values) => {
            if (!err) {
              this.state.smsSendBtn = true;
              let interval = window.setInterval(() => {
                if (that.state.time-- <= 0) {
                  that.state.time = 60;
                  that.state.smsSendBtn = false;
                  window.clearInterval(interval);
                }
              }, 1000);
              const hide = this.$message.loading('验证码发送中..', 0);
              const params = {
                mobile: values.mobile,
                smsmode: "1"
              };
              postAction("/sys/sms", params).then((res) => {
                if (!res.success) {
                  this.registerFailed(res.message);
                  setTimeout(hide, 0);
                }
                setTimeout(hide, 500);
              }).catch(err => {
                setTimeout(hide, 1);
                clearInterval(interval);
                that.state.time = 60;
                that.state.smsSendBtn = false;
                this.requestFailed(err);
              });
            }
          }
        );
      },
      registerFailed(message) {
        this.$notification['error']({
          message: "注册失败",
          description: message,
          duration: 2,
        });

      },
      requestFailed(err) {
        this.$notification['error']({
          message: '错误',
          description: ((err.response || {}).data || {}).message || "请求出现错误，请稍后再试",
          duration: 4,
        });
        this.registerBtn = false;
      },
    },
    watch: {
      'state.passwordLevel'(val) {
        console.log(val)

      }
    }
  }
</script>
<style lang="scss">
  .user-register {

    &.error {
      color: #ff0000;
    }

    &.warning {
      color: #ff7e05;
    }

    &.success {
      color: #52c41a;
    }

  }

  .user-layout-register {
    .ant-input-group-addon:first-child {
      background-color: #fff;
    }
  }
</style>
<style lang="scss" scoped>
  .user-layout-register {

    & > h3 {
      font-size: 16px;
      margin-bottom: 20px;
      /*margin-top: 0px;*/
    }

    .getCaptcha {
      display: block;
      width: 100%;
      height: 40px;
    }

    .register-button {
      width: 50%;
    }

    .reload-button {
      width: 40%;
    }

    .login {
      float: right;
      line-height: 40px;
    }
  }
</style>