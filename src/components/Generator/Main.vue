<template>
  <div>
    <div class="fd-nav">
      <div class="fd-nav-left">
        <div class="fd-nav-back">
          <i aria-label="icon: left" class="anticon anticon-left"
            ><svg viewBox="64 64 896 896" focusable="false" class="" data-icon="left" width="1em" height="1em" fill="currentColor" aria-hidden="true">
              <path
                d="M724 218.3V141c0-6.7-7.7-10.4-12.9-6.3L260.3 486.8a31.86 31.86 0 0 0 0 50.3l450.8 352.1c5.3 4.1 12.9.4 12.9-6.3v-77.3c0-4.9-2.3-9.6-6.1-12.6l-360-281 360-281.1c3.8-3 6.1-7.7 6.1-12.6z"
              /></svg
          ></i>
        </div>
      </div>
      <div class="fd-nav-center">
        <div class="fd-nav-container">
          <div class="fd-nav-item"><span class="order-num">1</span>流程设计</div>
        </div>
      </div>
      <div class="fd-nav-right">
        <button type="button" class="ant-btn button-preview" @click="preview">
          <span>预览</span>
        </button>
        <button type="button" class="ant-btn button-preview" @click="save">
          <span>发布</span>
        </button>
      </div>
    </div>
    <div class="fd-nav-content">
      <div class="dingflow-design">
        <!-- <div class="zoom">
          <div class="zoom-out" />
          <span>100%</span>
          <div class="zoom-in" />
        </div> -->
        <div v-if="!editState" class="ie-polyfill-container">
          <div id="box-scale" :key="key" class="box-scale" style="transform: scale(1); transform-origin: 50% 0px 0px;">
            <Node v-for="(item, index) in items" :key="index" :node="item" :editState="editState" @addnode="addnode" @delNode="delNode(item)" />
            <EndNode />
          </div>
        </div>

        <div v-if="editState" class="ie-polyfill-container">
          <div id="box-scale" :key="key" class="box-scale" style="transform: scale(1); transform-origin: 50% 0px 0px;">
            <Node v-for="(item, index) in items" :key="index" :node="item" :editState="editState" @addnode="addnode" @delNode="delNode(item)" />
            <EndNode />
            <AModal :dialog.sync="viewModal">
              <pre style="font-family: Monaco,Menlo,Consolas,Bitstream Vera Sans Mono,monospace;font-size: 14px;">{{ JSON.stringify(data1.node, null, 4) }}</pre>
            </AModal>
            <ErrorsModal :dialog.sync="errorsModal" :data="errors" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import AModal from './../AModal/AModal'
import EndNode from './end-node'
import ErrorsModal from './errors-modal'
import { iteratorData, addNewNode, delNode, checkData } from './process'
export default {
  name: 'WorkflowUi',
  components: {
    EndNode,
    AModal,
    ErrorsModal
  },
  props: {
    editState: {
      type: Boolean,
      default: () => {
        return false
      }
    },
    data: {
      type: Object,
      default: undefined
    }
  },
  data: () => ({
    items: [],
    key: 0,
    errorsModal: false,
    errors: [],
    viewModal: false,
    data1: {}
  }),
  watch: {
    data: {
      handler(val) {
        // this.reverseData(val)
        this.data1 = val
      },
      deep: true,
      immediate: true
    }
  },
  mounted() {
    if (this.data && this.data.node) {
      this.data1 = this.data
    }
    // 暂时隐藏
    if (!this.data1.node && this.editState) {
      this.initialNode()
    }
    if (!this.editState) {
      // console.log(this.data,'侧事故')
      this.reverseData(this.data)
    }
    // this.iteratorData(this.data1.node)
  },
  methods: {
    initialNode() {
      this.data1.node = {
        name: '测试',
        type: 'start',
        nodeId: 'sid-startevent'
      }
    },
    reverseData(data) {
      if (data && !this.editState) {
        this.items.push(data)
        if (data.childNode !== null && data.childNode !== undefined) {
          this.reverseData(data.childNode)
        }
      }
    },
    iteratorData(data) {
      this.items = []
      iteratorData(this.items, data)
    },
    addnode(node) {
      // console.log('添加节点:' + node.nodeId)
      addNewNode(node, this.data1.node, this.items)
      console.log(node, this.data1.node, this.items, '测试')
      this.key++
    },
    delNode(node) {
      delNode(node, this.data1.node, this.items)
      this.key++
      // this.iteratorData(this.data1.node)
    },
    save() {
      var errors = checkData(this.data1.node)
      if (errors.length > 0) {
        this.errorsModal = true
        this.errors = errors
        return
      }
      this.$emit('ok', this.data1)
      console.log(this.data1)
    },
    preview() {
      var errors = checkData(this.data1.node)
      if (errors.length > 0) {
        this.errorsModal = true
        this.errors = errors
        return
      }
      this.viewModal = true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
