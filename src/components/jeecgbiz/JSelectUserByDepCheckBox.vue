<template>
  <div>
    <!--<a-input-search-->
      <!--v-model="selectedDepUsers"-->
      <!--placeholder="请先选择用户"-->
      <!--disabled-->
      <!--@search="onSearchDepUser">-->
      <!--<a-button slot="enterButton" :disabled="disabled">选择用户</a-button>-->
    <!--</a-input-search>-->
    <j-select-user-by-dep-modal-check-box
      ref="selectModal"
      @getUserId="getUserId"
      :modal-width="modalWidth"
      @ok="onSearchDepUserCallBack"/>
  </div>
</template>

<script>
  import JSelectUserByDepModalCheckBox from './modal/JSelectUserByDepModalCheckBox'

  export default {
    name: 'JSelectUserByDepCheckBox',
    components: {JSelectUserByDepModalCheckBox},
    props: {
      modalWidth: {
        type: Number,
        default: 1250,
        required: false
      },
      value: {
        type: String,
        required: false
      },
      disabled: {
        type: Boolean,
        required: false,
        default: false
      }
    },
    data() {
      return {
        selectedDepUsers: "",
      }
    },
    mounted() {
      this.selectedDepUsers = this.value
    },
    watch: {
      value(val) {
        this.selectedDepUsers = val
      }
    },
    model: {
      prop: 'value',
      event: 'change'
    },
    methods: {
      getUserId(data) {
        this.$emit('senUserId', data,this.selectedDepUsers);

        this.$emit('getUD2')
      },
      getSelectUsers() {
        return this.$refs.selectModal.chooseUserId
      },
      //通过组织机构筛选选择用户
      onSearchDepUser() {
        this.$refs.selectModal.showModal()
        this.onSearchDepUserCallBack('')
      },
      show() {
        this.$refs.selectModal.showModal()
        this.onSearchDepUserCallBack('')
      },
      onSearchDepUserCallBack(selectedDepUsers) {
        this.selectedDepUsers = selectedDepUsers
        this.$emit("change", selectedDepUsers)

      }
    }
  }
</script>

<style scoped>

</style>