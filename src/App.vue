<template>
  <div id="app">
    <Main :data.sync="data" :editState="editState" @ok="ok" />
    <div style="height:300px"></div>
  </div>
</template>

<script>
import Main from './components/Generator/Main'
export default {
  name: 'App',
  components: {
    Main
  },
  props: {
    data: {
      type: Object,
      default: () => {
        return {
          name: '测试',
          type: 'start',
          nodeId: 'sid-startevent',
          childNode: {
            type: 'route',
            prevId: 'sid-startevent',
            nodeId: '1604391500845',
            conditionNodes: [
              {
                name: '条件1',
                type: 'condition',
                prevId: '1604391500845',
                nodeId: '1604391500855',
                properties: {
                  conditions: [
                    [
                      {
                        pos: 1,
                        type: 'dingtalk_actioner_range_condition',
                        paramKey: 'day',
                        paramLabel: '请假天数',
                        key: 'lt',
                        label: '小于',
                        upperBound: '1'
                      }
                    ]
                  ]
                }
              },
              {
                name: '条件2',
                type: 'condition',
                prevId: '1604391500855',
                nodeId: '1604391500865',
                properties: {
                  conditions: [
                    [
                      {
                        pos: 1,
                        type: 'dingtalk_actioner_range_condition',
                        paramKey: 'day',
                        paramLabel: '请假天数',
                        key: 'ge',
                        label: '大于等于',
                        boundEqual: '',
                        upperBound: '',
                        upperBoundEqual: '',
                        lowerBound: '',
                        lowerBoundEqual: '1'
                      },
                      {
                        pos: 2,
                        type: 'dingtalk_actioner_range_condition',
                        paramKey: 'day',
                        paramLabel: '请假天数',
                        key: 'le',
                        label: '小于等于',
                        upperBoundEqual: '7'
                      }
                    ]
                  ]
                },
                childNode: {
                  name: '抄送人',
                  prevId: '1604391500865',
                  nodeId: '1604391764038',
                  type: 'notifier',
                  childNode: {
                    type: 'route',
                    prevId: '1604391764038',
                    nodeId: '1604391664609',
                    conditionNodes: [
                      {
                        name: '条件1',
                        type: 'condition',
                        prevId: '1604391664609',
                        nodeId: '1604391664619',
                        properties: {
                          conditions: [
                            [
                              {
                                pos: 1,
                                type: 'dingtalk_actioner_range_condition',
                                paramKey: 'day',
                                paramLabel: '请假天数',
                                key: 'eq',
                                label: '等于',
                                boundEqual: '1'
                              }
                            ]
                          ]
                        }
                      },
                      {
                        name: '条件2',
                        type: 'condition',
                        prevId: '1604391664619',
                        nodeId: '1604391664629',
                        properties: {
                          conditions: [
                            [
                              {
                                pos: 1,
                                type: 'dingtalk_actioner_range_condition',
                                paramKey: 'day',
                                paramLabel: '请假天数',
                                key: 'gt',
                                label: '大于',
                                lowerBound: '1'
                              },
                              {
                                pos: 2,
                                type: 'dingtalk_actioner_range_condition',
                                paramKey: 'day',
                                paramLabel: '请假天数',
                                key: 'lt',
                                label: '小于',
                                upperBound: '7'
                              }
                            ]
                          ]
                        }
                      },
                      {
                        name: '条件3',
                        type: 'condition',
                        prevId: '1604391664619',
                        nodeId: '1604391666036',
                        properties: {
                          conditions: [
                            [
                              {
                                pos: 1,
                                type: 'dingtalk_actioner_range_condition',
                                paramKey: 'day',
                                paramLabel: '请假天数',
                                key: 'eq',
                                label: '等于',
                                boundEqual: '7'
                              }
                            ]
                          ]
                        }
                      }
                    ]
                  },
                  properties: {
                    actionerRules: [
                      {
                        type: 'target_label',
                        labelNames: 'test',
                        labels: '',
                        isEmpty: false,
                        memberCount: 1,
                        actType: 'or'
                      }
                    ]
                  }
                }
              },
              {
                name: '条件3',
                type: 'condition',
                prevId: '1604391500855',
                nodeId: '1604391521143',
                properties: {
                  conditions: [
                    [
                      {
                        pos: 1,
                        type: 'dingtalk_actioner_range_condition',
                        paramKey: 'day',
                        paramLabel: '请假天数',
                        key: 'gt',
                        label: '大于',
                        lowerBound: '7'
                      }
                    ]
                  ]
                },
                childNode: {
                  name: '审批人',
                  prevId: '1604391521143',
                  nodeId: '1604391753405',
                  type: 'approver',
                  properties: {
                    actionerRules: [
                      {
                        type: 'target_management',
                        level: 1,
                        isEmpty: false,
                        autoUp: true,
                        actType: 'or'
                      }
                    ]
                  }
                }
              }
            ],
            childNode: {
              name: '抄送人',
              prevId: '1604391500845',
              nodeId: '1604391361242',
              type: 'notifier',
              childNode: {
                name: '审批人',
                prevId: '1604391361242',
                nodeId: '1604391363670',
                type: 'approver',
                properties: {
                  actionerRules: [
                    {
                      type: 'target_label',
                      labelNames: '杨',
                      labels: '',
                      isEmpty: false,
                      memberCount: 1,
                      actType: 'or'
                    }
                  ]
                }
              },
              properties: {
                actionerRules: [
                  {
                    type: 'target_management',
                    level: 1,
                    isEmpty: false,
                    autoUp: true,
                    actType: 'or'
                  }
                ]
              }
            }
          }
        }
      }
    },
    editState: {
      type: Boolean,
      default: () => {
        return false
      }
    }
  },
  methods: {
    ok(data) {
      this.$emit('update:data', data)
    }
  }
}
</script>

<style>
#app {
  position: relative;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
