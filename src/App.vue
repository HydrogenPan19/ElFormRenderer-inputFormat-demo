<template>
  <div id="app">

    <ElRow>
      <span>update status:</span>
      <ElButton @click="updateStatus(Status.normal)">normal</ElButton>
      <ElButton @click="updateStatus(Status.blocked)">blocked</ElButton>
      <ElButton @click="updateStatus(Status.limited)">limited</ElButton>
    </ElRow>

    <ElRow>
      <ElButton @click="refreshValue">refreshValue</ElButton>
      <span>switchValue: <span>{{ switchValue }}</span></span>
    </ElRow>

    <ElDivider>form</ElDivider>
    <ElFormRenderer
        :ref="formName"
        :content="formContent"
        label-width="120px"
    />
  </div>
</template>

<script>
  import ElFormRenderer from '@femessage/el-form-renderer'

  const Status = {
    normal: 0,
    blocked: 1,
    limited: 2,
    // ... other status code
  }
  Object.freeze(Status)

export default {
  name: 'app',
  components: {
    ElFormRenderer
  },

  data() {
    const formContentValue = () => {
      return [
        {
          id: 'status',
          label: '是否启用',
          type: 'switch',
          el: {
            activeText: '启用',
            activeValue: true,
            inactiveText: '不启用',
            inactiveValue: false,
          },
          inputFormat(data) {
            return data.status === Status.normal
          },
          outputFormat(value) {
            // eslint-disable-next-line no-console
            console.log('outputFormat origin value', value)
            if (typeof value !== 'boolean') {
              // eslint-disable-next-line no-console
              console.warn('The switch value should be boolean')
            }

            return value
              ? Status.normal
              : Status.blocked
          },
        }
      ]
    }

    return {
      formName: 'form',

      Status,

      formContent: formContentValue(),

      switchValue: false,
    }
  },

  mounted() {
    this.updateStatus(Status.normal)
  },

  methods: {
    getFormRef() {
      return this.$refs[this.formName]
    },

    updateStatus(status) {
      this.getFormRef()
        .updateForm({
          status,
        })

      this.$nextTick(() => {
        this.refreshValue()
      })
    },

    refreshValue() {
      const formData = this.getFormRef().getFormValue()
      this.switchValue = formData.status
    },
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
