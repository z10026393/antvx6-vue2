<template>
  <div class="node-panel">
    <div class="panel-title">基础节点</div>
    <div
      v-for="node in basicNodes"
      :key="node.shape"
      class="node-item"
      draggable="true"
      @dragstart="onDragStart($event, node)"
    >
      {{ node.label }}
    </div>
    
    <div class="panel-title custom-title">自定义节点</div>
    <div
      v-for="node in customNodes"
      :key="node.shape"
      class="node-item custom-item"
      draggable="true"
      @dragstart="onDragStart($event, node)"
    >
      {{ node.label }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'NodePanel',
  data() {
    return {
      basicNodes: [
        { shape: 'rect', label: '矩形节点' },
        { shape: 'circle', label: '圆形节点' },
        { shape: 'ellipse', label: '椭圆节点' }
      ],
      customNodes: [
        { 
          shape: 'custom-node',
          label: '流程节点',
          defaultData: {
            title: '流程节点',
            items: ['步骤1', '步骤2', '步骤3'],
            status: '进行中',
            color: '#722ed1'
          }
        },
        {
          shape: 'custom-node',
          label: '成功节点',
          defaultData: {
            title: '成功节点',
            items: ['步骤A', '步骤B'],
            status: '成功',
            color: '#52c41a'
          }
        }
      ]
    }
  },
  methods: {
    onDragStart(e, node) {
      e.dataTransfer.setData('text/plain', JSON.stringify(node))
    }
  }
}
</script>

<style scoped>
.node-panel {
  width: 200px;
  padding: 10px;
  background: #fafafa;
  border-right: 1px solid #e8e8e8;
  overflow-y: auto;
}

.panel-title {
  padding: 8px 0;
  font-weight: bold;
  color: #595959;
}

.custom-title {
  margin-top: 15px;
}

.node-item {
  padding: 8px 12px;
  margin-bottom: 8px;
  background: #fff;
  border: 1px solid #d9d9d9;
  border-radius: 4px;
  cursor: move;
  user-select: none;
  font-size: 12px;
}

.custom-item {
  border-left: 3px solid #722ed1;
}

.node-item:hover {
  background: #e6f7ff;
  border-color: #91d5ff;
}
</style>