<template>
  <div class="flow-container">
    <div class="toolbar">
      <h3>节点面板</h3>
      <div class="node-item" draggable="true" @dragstart="onDragStart('custom-node-shape', $event)">
        自定义节点
      </div>
      <div class="action-buttons">
        <button @click="saveGraph">保存流程图</button>
        <button @click="loadGraph">加载流程图</button>
        <button @click="clearGraph">清空画布</button>
      </div>
    </div>
    <div ref="container" class="flow-chart"></div>

    <!-- 右键菜单 -->
    <div
      v-if="contextMenu.visible"
      class="context-menu"
      :style="{ left: contextMenu.x + 'px', top: contextMenu.y + 'px' }"
      @click.stop
    >
      <div v-if="contextMenu.node" class="menu-item" @click="deleteNode">
        <span class="icon">🗑️</span> 删除节点
      </div>
      <div v-if="contextMenu.node" class="menu-item" @click="editNode">
        <span class="icon">✏️</span> 编辑节点
      </div>
      <div class="menu-item" @click="addNode"><span class="icon">➕</span> 添加节点</div>
      <div class="menu-divider"></div>
      <div class="menu-item" @click="hideContextMenu"><span class="icon">❌</span> 取消</div>
    </div>
  </div>
</template>

<script>
import { Graph, Shape, Addon } from "@antv/x6";
import { Export } from "@antv/x6-plugin-export";
import { Transform } from "@antv/x6-plugin-transform";
import { Snapline } from "@antv/x6-plugin-snapline";
import { Clipboard } from "@antv/x6-plugin-clipboard";
import { Keyboard } from "@antv/x6-plugin-keyboard";
import { Selection } from "@antv/x6-plugin-selection";
import { History } from "@antv/x6-plugin-history";
import { Dnd } from "@antv/x6-plugin-dnd";
import { register } from "@antv/x6-vue-shape";
import CustomNode from "./CustomNode.vue";
var _this = null;
export default {
  name: "FlowChart",
  components: { CustomNode },
  data() {
    return {
      graph: null,
      dnd: null,
      contextMenu: {
        visible: false,
        x: 0,
        y: 0,
        node: null,
      },
      customNodeData: {
        shape: "custom-node-shape",
        component: "custom-node-shape",
        data: {
          title: "自定义节点",
          items: ["项目1", "项目2", "项目3"],
        },
        width: 200,
        height: 80,
        ports: {
          groups: {
            top: {
              position: "top",
              attrs: {
                circle: {
                  r: 4,
                  magnet: true,
                  stroke: "#5F95FF",
                  strokeWidth: 1,
                  fill: "#fff",
                },
              },
            },
            bottom: {
              position: "bottom",
              attrs: {
                circle: {
                  r: 4,
                  magnet: true,
                  stroke: "#5F95FF",
                  strokeWidth: 1,
                  fill: "#fff",
                },
              },
            },
            left: {
              position: "left",
              attrs: {
                circle: {
                  r: 4,
                  magnet: true,
                  stroke: "#5F95FF",
                  strokeWidth: 1,
                  fill: "#fff",
                },
              },
            },
            right: {
              position: "right",
              attrs: {
                circle: {
                  r: 4,
                  magnet: true,
                  stroke: "#5F95FF",
                  strokeWidth: 1,
                  fill: "#fff",
                },
              },
            },
          },
          items: [
            { id: "top", group: "top" },
            { id: "bottom", group: "bottom" },
            { id: "left", group: "left" },
            { id: "right", group: "right" },
          ],
        },
      },
    };
  },
  mounted() {
    _this = this;
    this.initGraph();
    this.initDnd();
    this.loadGraph();

    // 点击其他地方隐藏菜单
    document.addEventListener("click", this.hideContextMenu);
  },
  beforeDestroy() {
    if (this.graph) {
      this.graph.dispose();
    }
    if (this.dnd) {
      this.dnd.dispose();
    }
    document.removeEventListener("click", this.hideContextMenu);
  },
  methods: {
    initGraph() {
      // 注册Vue组件节点
      register({
        shape: "custom-node-shape",
        width: 200,
        height: 80,
        component: CustomNode,
      });

      // 注册自定义边
      Graph.registerEdge(
        "custom-edge",
        {
          inherit: "edge",
          attrs: {
            line: {
              stroke: "#1890ff",
              strokeWidth: 2,
              targetMarker: {
                name: "block",
                width: 12,
                height: 8,
              },
            },
          },
          zIndex: 0,
        },
        true
      );

      // 创建画布
      this.graph = new Graph({
        container: this.$refs.container,
        width: "100%",
        height: "100%",
        grid: {
          visible: true,
          type: "doubleMesh",
          args: [
            {
              color: "#eee",
              thickness: 1,
            },
            {
              color: "#ddd",
              thickness: 1,
              factor: 4,
            },
          ],
        },
        panning: {
          enabled: true,
          modifiers: "shift",
        },
        mousewheel: {
          enabled: true,
          zoomAtMousePosition: true,
          modifiers: ["ctrl", "meta"],
          minScale: 0.5,
          maxScale: 3,
        },
        connecting: {
          router: {
            name: "manhattan",
            args: {
              padding: 10,
            },
          },
          connector: {
            name: "rounded",
            args: {
              radius: 8,
            },
          },
          anchor: "center",
          connectionPoint: "anchor",
          allowBlank: false,
          allowEdge: false,
          snap: {
            radius: 50,
          },
          createEdge() {
            return new Shape.Edge({
              shape: "custom-edge",
              attrs: {
                line: {
                  stroke: "#1890ff",
                  strokeWidth: 2,
                },
              },
            });
          },
          validateConnection({ sourceView, targetView, sourceMagnet, targetMagnet }) {
            // 不允许连接到自身
            if (sourceView === targetView) {
              return false;
            }

            // 限制连接到目标节点，是否有其他节点连接
            const edges = _this.graph.getOutgoingEdges(targetView.cell);
            if (edges && edges.length > 0) {
              return false;
            }
            return true;
          },
        },
        highlighting: {
          magnetAdsorbed: {
            name: "stroke",
            args: {
              attrs: {
                fill: "#5F95FF",
                stroke: "#5F95FF",
              },
            },
          },
        },
        interacting: {
          edgeLabelMovable: true,
        },
      });

      // 启用插件
      this.graph.use(
        new Selection({
          enabled: true,
          rubberband: true,
          showNodeSelectionBox: true,
          pointerEvents: "none",
        })
      );

      // this.graph.use(
      //   new Snapline({
      //     enabled: true,
      //     sharp: true,
      //   })
      // );

      // this.graph.use(
      //   new Transform({
      //     resizing: {
      //       enabled: true,
      //       minWidth: 100,
      //       minHeight: 40,
      //       maxWidth: 800,
      //       maxHeight: 600,
      //       orthogonal: false,
      //     },
      //     rotating: {
      //       enabled: true,
      //       grid: 15,
      //     },
      //   })
      // );

      // 事件监听
      this.graph.on("node:dblclick", ({ node }) => this.editNode(node));
      this.graph.on("node:contextmenu", ({ e, node }) => this.showContextMenu(e, node));
      this.graph.on("blank:contextmenu", ({ e }) => this.showBlankContextMenu(e));
      this.graph.on(
        "edge:connected",
        ({ e, edge, view, isNew, previousCell, previousView, currentCell, currentView }) => {
          console.log(previousCell, previousView, currentCell, currentView);
        }
      );
    },

    initDnd() {
      this.dnd = new Dnd({
        target: this.graph,
        getDragNode: (node) => node.clone(),
        getDropNode: (node) => node.clone(),
        validateNode(droppingNode) {
          return droppingNode.shape === "custom-node-shape";
        },
      });
    },

    onDragStart(type, e) {
      e.preventDefault();
      let node;
      if (type === "custom-node-shape") {
        node = this.graph.createNode({
          ...this.customNodeData,
          data: {
            title: "节点1",
          },
        });
      }
      this.dnd.start(node, e);
    },
    updateNodeData() {
      const node = this.graph.getCellById("node1");
      node.updateData({
        title: "更新后的节点",
        items: ["新项目1", "新项目2"],
      });
    },
    handleNodeClick(data) {
      console.log("节点被点击:", data);
    },
    showContextMenu(e, node) {
      e.preventDefault();
      this.contextMenu = {
        visible: true,
        x: e.clientX,
        y: e.clientY,
        node,
      };
    },

    showBlankContextMenu(e) {
      e.preventDefault();
      this.contextMenu = {
        visible: true,
        x: e.clientX,
        y: e.clientY,
        node: null,
      };
    },

    hideContextMenu() {
      this.contextMenu.visible = false;
    },

    deleteNode() {
      if (this.contextMenu.node) {
        this.contextMenu.node.remove();
      }
      this.hideContextMenu();
    },

    editNode(node) {
      const targetNode = this.contextMenu.node;
      if (targetNode) {
        const data = targetNode.getData();
        const title = prompt("请输入节点标题", data.title || "节点");
        if (title !== null) {
          targetNode.setData({ ...data, title });
        }
      }
      this.hideContextMenu();
    },

    addNode() {
      const { x, y } = this.graph.clientToLocal(
        this.contextMenu.x - this.$refs.container.getBoundingClientRect().left,
        this.contextMenu.y - this.$refs.container.getBoundingClientRect().top
      );

      const node = this.graph.createNode({
        shape: "custom-node-shape",
        component: "custom-node-shape",
        x,
        y,
        data: {
          title: "新节点",
          items: ["项目A", "项目B"],
        },
        width: 200,
        height: 120,
        ports: {
          groups: {
            top: { position: "top", attrs: { circle: { r: 4, magnet: true } } },
            bottom: { position: "bottom", attrs: { circle: { r: 4, magnet: true } } },
          },
          items: [
            { id: "top", group: "top" },
            { id: "bottom", group: "bottom" },
          ],
        },
      });

      this.graph.addNode(node);
      this.hideContextMenu();
    },

    saveGraph() {
      try {
        const data = this.graph.toJSON();
        localStorage.setItem("flow-chart-data", JSON.stringify(data));
        alert("流程图保存成功!");
      } catch (error) {
        console.error("保存失败:", error);
        alert("流程图保存失败!");
      }
    },

    loadGraph() {
      // try {
      //   const data = localStorage.getItem("flow-chart-data");
      //   if (data) {
      //     this.graph.fromJSON(JSON.parse(data));
      //     alert("流程图加载成功!");
      //   } else {
      //     alert("没有找到保存的流程图数据!");
      //   }
      // } catch (error) {
      //   console.error("加载失败:", error);
      //   alert("流程图加载失败!");
      // }
    },

    clearGraph() {
      if (confirm("确定要清空画布吗？")) {
        this.graph.clearCells();
      }
    },
  },
};
</script>

<style scoped>
.flow-container {
  display: flex;
  height: 100vh;
  width: 100%;
  overflow: hidden;
}

.toolbar {
  width: 250px;
  padding: 15px;
  background: #f5f5f5;
  border-right: 1px solid #ddd;
  display: flex;
  flex-direction: column;
}

.toolbar h3 {
  margin: 0 0 15px 0;
  padding-bottom: 10px;
  border-bottom: 1px solid #ddd;
}

.node-item {
  padding: 10px 15px;
  margin-bottom: 10px;
  background: white;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: move;
  user-select: none;
  transition: all 0.2s;
}

.node-item:hover {
  background: #f0f0f0;
  border-color: #1890ff;
  box-shadow: 0 2px 8px rgba(24, 144, 255, 0.2);
}

.action-buttons {
  margin-top: auto;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.action-buttons button {
  padding: 8px 12px;
  background: #1890ff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

.action-buttons button:hover {
  background: #40a9ff;
}

.flow-chart {
  flex: 1;
  height: 100%;
  background-color: #fafafa;
}

.context-menu {
  position: fixed;
  background: white;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  z-index: 1000;
  min-width: 150px;
}

.menu-item {
  padding: 8px 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
}

.menu-item:hover {
  background: #f5f5f5;
}

.menu-divider {
  height: 1px;
  background: #eee;
  margin: 4px 0;
}

.icon {
  font-size: 14px;
}
</style>
