<template>
  <div id="app">
    <bpmn-modeler
      ref="refNode"
      :xml="xml"
      :users="users"
      :groups="groups"
      :categorys="categorys"
      :is-view="false"
    />
    <el-button type="primary" @click="save">保存</el-button>
  </div>
</template>

<script>
import bpmnModeler from '../package/index'
export default {
  components: {
    bpmnModeler
  },
  data() {
    return {
      xml: '', // 后端查询到的xml
      users: [
        { name: '张三', id: 'zhangsan' },
        { name: '李四', id: 'lisi' },
        { name: '王五', id: 'wangwu' }
      ],
      groups: [
        { name: 'web组', id: 'web' },
        { name: 'java组', id: 'java' },
        { name: 'python组', id: 'python' }
      ],
      categorys: [
        { name: 'OA', id: 'oa' },
        { name: '财务', id: 'finance' }
      ]
    }
  },
  mounted() {
    this.getModelDetail()
  },
  methods: {
    getModelDetail() {
      fetch('https://cdn.jsdelivr.net/gh/goldsubmarine/workflow-bpmn-modeler@master/src/Leave.bpmn20.xml')
        .then(response => {
          return response.text()
        }).then(xml => {
          this.xml = xml
        })
    },
    async save() {
      const processModel = this.$refs['refNode'].getProcess()
      const xml = await this.$refs['refNode'].saveXML()
      const svg = await this.$refs['refNode'].saveImg()
      console.log(processModel, xml, svg)
    }
  }
}
</script>

<style lang="scss">
html, body, #app {
  height: 650px;
  margin: 0;
}
</style>
