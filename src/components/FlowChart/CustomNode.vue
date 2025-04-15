<template>
  <div class="custom-node" @click="handleClick">
    <h3>{{ nodeData.title }}</h3>
    <!-- <button @click="updateTitle">修改标题</button> -->
  </div>
</template>

<script>
export default {
  name: "CustomNode",
  inject: ["getNode"],
  data() {
    return {
      nodeData: {
        title: "按钮修改的标题",
      },
    };
  },
  mounted() {
    this.nodeData = this.getNode().data;
    // 监听节点数据变化
    this.getNode().on("change:data", ({ current }) => {
      this.nodeData = current;
    });
  },
  methods: {
    handleClick() {
      this.$emit("node-click", this.nodeData);
    },
    updateTitle() {
      if (this.graph) {
        const node = this.graph.getSelectedCells()[0];
        if (node) {
          node.updateData({
            title: "按钮修改的标题",
          });
        }
      }
    },
  },
};
</script>

<style scoped>
.custom-node {
  width: 200px;
  height: 80px;
  background: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.node-header {
  padding: 8px 12px;
  background: #1890ff;
  color: white;
  font-weight: bold;
  text-align: center;
}

.node-content {
  padding: 8px;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.node-item {
  padding: 4px 0;
  border-bottom: 1px dashed #eee;
  font-size: 12px;
}

.node-item:last-child {
  border-bottom: none;
}
</style>
