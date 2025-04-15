<template>
  <div class="processBox">
    <!-- 流程图 -->
    <div id="container" style="min-width: 600px; min-height: 600px">
      <div id="stencil"></div>
      <div id="graph-container"></div>
    </div>
    <div class="saveBtn">
      <el-button type="info" size="mini" @click="graphSave()">保存</el-button>
      <el-button type="danger" size="mini" @click="graphClear()">清空</el-button>
    </div>
  </div>
</template>
<script >
import { Graph, Shape, Addon, DataUri } from "@antv/x6";
import { Stencil } from "@antv/x6-plugin-stencil";
import { Transform } from "@antv/x6-plugin-transform";
import { Selection } from "@antv/x6-plugin-selection";
import { Snapline } from "@antv/x6-plugin-snapline";
import { Keyboard } from "@antv/x6-plugin-keyboard";
import { Clipboard } from "@antv/x6-plugin-clipboard";
import { History } from "@antv/x6-plugin-history";
import insertCss from "insert-css";
let graph = null;
let dnd = null;
let selector = null;
export default {
  name: "ProcessLibrary",
  components: {},
  data() {
    return {
      silkData: [],
      tobaccoData: [],
      stemData: [],
      smokeData: [],
      equipmentData: [],
      companyOptions: [],
      companyId: "",
      graphId: "",
      description: "",
      data: {},//渲染数据
      saveData: [],//保存数据
    };
  }, 
  watch: {
    data: {
      deep: true,
      immediate: true,
      handler(oldVal, newVal) {
        if (newVal) {
          graph.fromJSON(this.data);//渲染数据
        }
      },
    },
  },
  mounted() {
    this.getTreeList();
    this.companySearch();
    setTimeout(() => {
      this.initGraph();
    }, 500);
  },
  beforeDestroy() {
    // 画布的销毁以及回收
    graph && graph.dispose();
    graph = null;
    dnd = null;
    selector = null;
  },
  methods: {
    //建模树
    getTreeList() {},  
    //初始化
    initGraph() {
      graph = new Graph({
        container: document.getElementById("graph-container"),
        //画布背景
        background: {
          color: "#F2F7FA",
        },
        grid: true,
        // 滚动
        scroller: {
          enabled: true,
          pageVisible: false, // 是否分页
          pageBreak: false,
          pannable: true, // 是否平移
        },
        //滚轮缩放
        mousewheel: {
          enabled: true,
          zoomAtMousePosition: true,
          //   modifiers: "ctrl",
          //   minScale: 0.5,
          //   maxScale: 3,

          modifiers: ["ctrl", "meta"],
          maxScale: 3,
          minScale: 0.3,
        },
        //连接线
        connecting: {
          router: "manhattan",
          connector: {
            name: "rounded",
            args: {
              radius: 8,
            },
          },
          anchor: "center",
          connectionPoint: "anchor",
          allowBlank: false,
          snap: {
            radius: 20,
          },
          createEdge() {
            return new Shape.Edge({
              attrs: {
                line: {
                  stroke: "#A2B1C3",
                  strokeWidth: 2,
                  targetMarker: {
                    name: "block",
                    width: 12,
                    height: 8,
                  },
                },
              },
              zIndex: 0,
            });
          },
          validateConnection({ targetMagnet }) {
            return !!targetMagnet;
          },
        },
        //连接桩样式
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
      });
      // 使用插件
      graph
        .use(
          //变换
          new Transform({
            resizing: true, //调整大小
            rotating: true, //旋转角度
          })
        )
        .use(
          //框选
          new Selection({
            rubberband: true,
            showNodeSelectionBox: true,
          })
        )
        .use(new Snapline()) //对齐线
        .use(new Keyboard()) //快捷键
        .use(new Clipboard()) //复制粘贴
        .use(new History()); //撤销

      // 链接桩配置
      const ports = {
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
                style: {
                  visibility: "hidden",
                },
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
                style: {
                  visibility: "hidden",
                },
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
                style: {
                  visibility: "hidden",
                },
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
                style: {
                  visibility: "hidden",
                },
              },
            },
          },
        },
        items: [
          {
            group: "top",
          },
          {
            group: "right",
          },
          {
            group: "bottom",
          },
          {
            group: "left",
          },
        ],
      };
      //矩形设置
      Graph.registerNode(
        "custom-rect",
        {
          inherit: "rect",
          width: 72,
          height: 36,
          attrs: {
            body: {
              strokeWidth: 1,
              stroke: "#5F95FF",
              fill: "#EFF4FF",
            },
            text: {
              fontSize: 12,
              fill: "#262626",
            },
          },
          ports: { ...ports },
        },
        true
      );
      //设备矩形设置
      Graph.registerNode(
        "dev-rect",
        {
          inherit: "rect",
          width: 230,
          height: 36,
          attrs: {
            body: {
              strokeWidth: 1,
              stroke: "#5F95FF",
              fill: "#EFF4FF",
            },
            text: {
              fontSize: 12,
              fill: "#262626",
            },
          },
          ports: { ...ports },
        },
        true
      );
      //圆形设置
      Graph.registerNode(
        "custom-circle",
        {
          inherit: "circle",
          width: 45,
          height: 45,
          attrs: {
            body: {
              strokeWidth: 1,
              stroke: "#5F95FF",
              fill: "#EFF4FF",
              overflow: "hidden",
              whiteSpace: "nowrap",
              textOverflow: "ellipsis",
            },
            text: {
              fontSize: 12,
              fill: "#262626",
            },
          },
          ports: { ...ports },
        },
        true
      );
      // 初始化左侧
      this.graphNode();
      // 快捷键
      this.initEvent();
    },
    //左侧数据
    graphNode() {
      const stencil = new Stencil({
        title: "组件",
        target: graph,
        search: false,
        collapsable: true,
        stencilGraphWidth: 280,
        stencilGraphHeight: 180,
        //左侧标题
        groups: [
          {
            title: "..工艺",
            name: "group1",
            graphHeight: 140,
          },
          {
            title: "丝1",
            name: "group2",
            graphHeight: 420,
          },
          {
            title: "丝2",
            name: "group3",
            graphHeight: 300,
          },
          {
            title: "丝3",
            name: "group4",
            graphHeight: 150,
          },
          {
            title: "设备区",
            name: "group5",
            graphHeight: 600,
            layoutOptions: {
              columns: 1,
              columnWidth: 230,
            },
          },
        ],
        layoutOptions: {
          columns: 3,
          columnWidth: 80,
          rowHeight: 55,
        },
      });
      document.getElementById("stencil").appendChild(stencil.container);
      // 数据
      const rectNodes1 = this.silkData.map((item) =>
        graph.createNode({
          shape: "custom-rect",
          label: item.name,
          attrs: {
            body: {
              rx: 6,
              ry: 6,
            },
          },
        })
      );
      stencil.load(rectNodes1, "group1");  
      // 设备区数据
      const imageNodes = this.equipmentData.map((item) =>
        graph.createNode({
          shape: "dev-rect",
          label: item.name,
          attrs: {
            body: {
              rx: 6,
              ry: 6,
            },
          },
        })
      );
      stencil.load(imageNodes, "group5");
    },
    //快捷键与事件
    initEvent() {
      // 点击...
      graph.on("cell:click", (e) => {
        this.menuVisible = false;
        const { node } = e;
        const data = node.getData();
        console.log(data);
      });

      // Edge工具
      graph.on("cell:mouseenter", ({ cell }) => {
        if (cell.isEdge()) {
          cell.addTools([
            {
              name: "button-remove",
              args: {
                x: "30%",
                y: "50%",
              },
            },
          ]);
        }
      });
      graph.on("cell:mouseleave", ({ cell }) => {
        if (cell.isEdge()) {
          cell.removeTool("button-remove");
        }
      });

      // copy cut paste
      graph.bindKey(["meta+c", "ctrl+c"], () => {
        const cells = graph.getSelectedCells();
        if (cells.length) {
          graph.copy(cells);
        }
        return false;
      });
      graph.bindKey(["meta+x", "ctrl+x"], () => {
        const cells = graph.getSelectedCells();
        if (cells.length) {
          graph.cut(cells);
        }
        return false;
      });
      graph.bindKey(["meta+v", "ctrl+v"], () => {
        if (!graph.isClipboardEmpty()) {
          const cells = graph.paste({ offset: 32 });
          graph.cleanSelection();
          graph.select(cells);
        }
        return false;
      });

      //undo redo
      graph.bindKey(["meta+z", "ctrl+z"], () => {
        if (graph.history.canUndo()) {
          graph.history.undo();
        }
        return false;
      });
      graph.bindKey(["meta+shift+z", "ctrl+shift+z"], () => {
        if (graph.history.canRedo()) {
          graph.history.redo();
        }
        return false;
      });

      // select all
      graph.bindKey(["meta+shift+a", "ctrl+shift+a"], () => {
        const nodes = graph.getNodes();
        if (nodes) {
          graph.select(nodes);
        }
      });

      // delete
      graph.bindKey(["backspace", "delete"], () => {
        // 删除选中的元素
        const cells = graph.getSelectedCells();
        if (cells.length) {
          graph.removeCells(cells);
        }
      });

      // zoom
      graph.bindKey(["ctrl+1", "meta+1"], () => {
        const zoom = graph.zoom();
        if (zoom < 1.5) {
          graph.zoom(0.1);
        }
      });
      graph.bindKey(["ctrl+2", "meta+2"], () => {
        const zoom = graph.zoom();
        if (zoom > 0.5) {
          graph.zoom(-0.1);
        }
      });

      // 链接桩控制
      const showPorts = (ports, show) => {
        for (let i = 0, len = ports.length; i < len; i += 1) {
          ports[i].style.visibility = show ? "visible" : "hidden";
        }
      };
      graph.on("node:mouseenter", () => {
        const container = document.getElementById("graph-container");
        const ports = container.querySelectorAll(".x6-port-body");
        showPorts(ports, true);
      });
      graph.on("node:mouseleave", () => {
        const container = document.getElementById("graph-container");
        const ports = container.querySelectorAll(".x6-port-body");
        // if (this.isPortsShow) return
        showPorts(ports, false);
      });
    }, 
    //保存
    graphSave() {
      this.saveData = graph.toJSON();
      const saveDataTrans = JSON.stringify(this.saveData); 
    },
    //清空
    graphClear() {
      graph.clearCells();
    },
  },
};
</script> 
