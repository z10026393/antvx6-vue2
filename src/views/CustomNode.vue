<template>
  <div
    class="custom-node"
    :style="nodeStyle"
    @click="handleClick"
    @mouseenter="handleMouseEnter"
    @mouseleave="handleMouseLeave"
  >
    <div class="node-header" :style="headerStyle">
      <span>{{ nodeData.title }}</span>
      <div class="node-actions">
        <i class="iconfont icon-edit action-icon" @click.stop="handleEdit"></i>
        <i class="iconfont icon-delete action-icon" @click.stop="handleDelete"></i>
      </div>
    </div>
    <div class="node-body">
      <div v-for="(item, index) in nodeData.items" :key="index" class="node-item">
        <i class="iconfont icon-right"></i>
        <span>{{ item }}</span>
      </div>
    </div>
    <div class="node-footer">
      <span class="status-tag" :style="statusStyle">{{ nodeData.status }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "CustomNode",
  props: {
    node: {
      type: Object,
      required: true,
    },
  },
  mounted() {
    console.log(this.node, "=====node======");
  },
  computed: {
    nodeData() {
      return (
        this.node.getData() || {
          title: "未命名节点",
          items: [],
          status: "待处理",
          color: "#1890ff",
        }
      );
    },
    nodeStyle() {
      const isSelected = this.node?.isSelected() || true;
      return {
        width: "100%",
        height: "100%",
        border: `2px solid ${this.nodeData.color}`,
        borderColor: isSelected ? "#f5222d" : this.nodeData.color,
        borderRadius: "4px",
        background: "#fff",
        display: "flex",
        flexDirection: "column",
        overflow: "hidden",
        cursor: "pointer",
        boxShadow: isSelected ? "0 0 10px rgba(0,0,0,0.2)" : "none",
        transition: "all 0.3s",
      };
    },
    headerStyle() {
      return {
        padding: "8px 12px",
        background: this.nodeData.color,
        color: "#fff",
        fontWeight: "bold",
        display: "flex",
        justifyContent: "space-between",
        alignItems: "center",
      };
    },
    statusStyle() {
      const status = this.nodeData.status || "待处理";
      const colorMap = {
        成功: "#52c41a",
        失败: "#f5222d",
        进行中: "#faad14",
        待处理: "#d9d9d9",
      };
      return {
        background: colorMap[status] || "#d9d9d9",
        color: "#fff",
        padding: "2px 8px",
        borderRadius: "10px",
        fontSize: "12px",
        display: "inline-block",
      };
    },
  },
  methods: {
    handleClick() {
      this.node.trigger("custom-node:click", { node: this.node });
    },
    handleMouseEnter() {
      this.node.setAttrByPath("body/strokeWidth", 3);
    },
    handleMouseLeave() {
      this.node.setAttrByPath("body/strokeWidth", 2);
    },
    handleEdit(e) {
      e.stopPropagation();
      this.node.trigger("custom-node:edit", { node: this.node });
    },
    handleDelete(e) {
      e.stopPropagation();
      this.node.trigger("custom-node:delete", { node: this.node });
    },
  },
};
</script>

<style scoped>
.custom-node {
  box-sizing: border-box;
}

.node-header {
  font-size: 14px;
}

.node-body {
  padding: 8px 12px;
  flex: 1;
}

.node-item {
  padding: 4px 0;
  border-bottom: 1px dashed #f0f0f0;
  display: flex;
  align-items: center;
}

.node-item .iconfont {
  margin-right: 8px;
  font-size: 12px;
}

.node-item:last-child {
  border-bottom: none;
}

.node-footer {
  padding: 8px;
  text-align: right;
  border-top: 1px solid #f0f0f0;
}

.node-actions {
  display: flex;
}

.action-icon {
  margin-left: 8px;
  font-size: 14px;
  cursor: pointer;
}

.action-icon:hover {
  color: #ffec3d;
}

.status-tag {
  display: inline-block;
}
</style>
