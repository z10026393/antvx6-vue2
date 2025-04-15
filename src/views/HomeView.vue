<template>
  <div>
    <el-button
      style="float: right; margin-top: -75px; margin-right: 80px"
      size="mini"
      type="primary"
      @click="saveFlow"
      >保存</el-button
    >
    <el-container class="process-edit-container">
      <!-- 顶部功能区域 -->
      <!--    <el-header>-->
      <!--    </el-header>-->
      <el-container>
        <!-- 左侧组件区域 -->
        <el-aside width="200px">
          <div class="process-component-list">
            <img
              src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyBpZD0ic3RhcnRfbm9kZSIgd2lkdGg9IjgwcHgiIGhlaWdodD0iODBweCIgdmlld0JveD0iMCAwIDgwIDgwIiB2ZXJzaW9uPSIxLjEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiPgogICAgPCEtLSBHZW5lcmF0b3I6IFNrZXRjaCA0OS4xICg1MTE0NykgLSBodHRwOi8vd3d3LmJvaGVtaWFuY29kaW5nLmNvbS9za2V0Y2ggLS0+CiAgICA8dGl0bGU+R3JvdXAgMjwvdGl0bGU+CiAgICA8ZGVzYz5DcmVhdGVkIHdpdGggU2tldGNoLjwvZGVzYz4KICAgIDxkZWZzPgogICAgICAgIDxjaXJjbGUgaWQ9InBhdGgtMSIgY3g9IjM2IiBjeT0iMzYiIHI9IjM2Ij48L2NpcmNsZT4KICAgICAgICA8ZmlsdGVyIHg9Ii05LjclIiB5PSItNi45JSIgd2lkdGg9IjExOS40JSIgaGVpZ2h0PSIxMTkuNCUiIGZpbHRlclVuaXRzPSJvYmplY3RCb3VuZGluZ0JveCIgaWQ9ImZpbHRlci0yIj4KICAgICAgICAgICAgPGZlT2Zmc2V0IGR4PSIwIiBkeT0iMiIgaW49IlNvdXJjZUFscGhhIiByZXN1bHQ9InNoYWRvd09mZnNldE91dGVyMSI+PC9mZU9mZnNldD4KICAgICAgICAgICAgPGZlR2F1c3NpYW5CbHVyIHN0ZERldmlhdGlvbj0iMiIgaW49InNoYWRvd09mZnNldE91dGVyMSIgcmVzdWx0PSJzaGFkb3dCbHVyT3V0ZXIxIj48L2ZlR2F1c3NpYW5CbHVyPgogICAgICAgICAgICA8ZmVDb21wb3NpdGUgaW49InNoYWRvd0JsdXJPdXRlcjEiIGluMj0iU291cmNlQWxwaGEiIG9wZXJhdG9yPSJvdXQiIHJlc3VsdD0ic2hhZG93Qmx1ck91dGVyMSI+PC9mZUNvbXBvc2l0ZT4KICAgICAgICAgICAgPGZlQ29sb3JNYXRyaXggdmFsdWVzPSIwIDAgMCAwIDAgICAwIDAgMCAwIDAgICAwIDAgMCAwIDAgIDAgMCAwIDAuMDQgMCIgdHlwZT0ibWF0cml4IiBpbj0ic2hhZG93Qmx1ck91dGVyMSI+PC9mZUNvbG9yTWF0cml4PgogICAgICAgIDwvZmlsdGVyPgogICAgPC9kZWZzPgogICAgPGcgaWQ9IlBhZ2UtMSIgc3Ryb2tlPSJub25lIiBzdHJva2Utd2lkdGg9IjEiIGZpbGw9Im5vbmUiIGZpbGwtcnVsZT0iZXZlbm9kZCI+CiAgICAgICAgPGcgaWQ9IuWfuuehgOa1geeoi+Wbvi0wMSIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTEwNi4wMDAwMDAsIC05My4wMDAwMDApIj4KICAgICAgICAgICAgPGcgaWQ9Ikdyb3VwLTIiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDExMC4wMDAwMDAsIDk1LjAwMDAwMCkiPgogICAgICAgICAgICAgICAgPGcgaWQ9Ik92YWwiPgogICAgICAgICAgICAgICAgICAgIDx1c2UgZmlsbD0iYmxhY2siIGZpbGwtb3BhY2l0eT0iMSIgZmlsdGVyPSJ1cmwoI2ZpbHRlci0yKSIgeGxpbms6aHJlZj0iI3BhdGgtMSI+PC91c2U+CiAgICAgICAgICAgICAgICAgICAgPHVzZSBmaWxsLW9wYWNpdHk9IjAuOTIiIGZpbGw9IiNGRkYyRTgiIGZpbGwtcnVsZT0iZXZlbm9kZCIgeGxpbms6aHJlZj0iI3BhdGgtMSI+PC91c2U+CiAgICAgICAgICAgICAgICAgICAgPGNpcmNsZSBzdHJva2U9IiNGRkMwNjkiIHN0cm9rZS13aWR0aD0iMSIgY3g9IjM2IiBjeT0iMzYiIHI9IjM1LjUiPjwvY2lyY2xlPgogICAgICAgICAgICAgICAgPC9nPgogICAgICAgICAgICAgICAgPHRleHQgaWQ9IuW8gOWni+iKgueCuSIgZm9udC1mYW1pbHk9IlBpbmdGYW5nU0MtUmVndWxhciwgUGluZ0ZhbmcgU0MiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSJub3JtYWwiIGxpbmUtc3BhY2luZz0iMTIiIGZpbGw9IiMwMDAwMDAiIGZpbGwtb3BhY2l0eT0iMC42NSI+CiAgICAgICAgICAgICAgICAgICAgPHRzcGFuIHg9IjEyIiB5PSI0MSI+5byA5aeL6IqC54K5PC90c3Bhbj4KICAgICAgICAgICAgICAgIDwvdGV4dD4KICAgICAgICAgICAgPC9nPgogICAgICAgIDwvZz4KICAgIDwvZz4KPC9zdmc+"
              data-type="node"
              data-shape="circle"
              data-size="72*72"
              data-color="#FA8C16"
              data-label="开始节点"
              @mousedown="addNodeToGraph"
            />
            <img
              draggable="false"
              src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iODhweCIgaGVpZ2h0PSI1NnB4IiB2aWV3Qm94PSIwIDAgODggNTYiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+CiAgICA8IS0tIEdlbmVyYXRvcjogU2tldGNoIDQ5LjEgKDUxMTQ3KSAtIGh0dHA6Ly93d3cuYm9oZW1pYW5jb2RpbmcuY29tL3NrZXRjaCAtLT4KICAgIDx0aXRsZT5Hcm91cDwvdGl0bGU+CiAgICA8ZGVzYz5DcmVhdGVkIHdpdGggU2tldGNoLjwvZGVzYz4KICAgIDxkZWZzPgogICAgICAgIDxyZWN0IGlkPSJwYXRoLTEiIHg9IjAiIHk9IjAiIHdpZHRoPSI4MCIgaGVpZ2h0PSI0OCIgcng9IjQiPjwvcmVjdD4KICAgICAgICA8ZmlsdGVyIHg9Ii04LjglIiB5PSItMTAuNCUiIHdpZHRoPSIxMTcuNSUiIGhlaWdodD0iMTI5LjIlIiBmaWx0ZXJVbml0cz0ib2JqZWN0Qm91bmRpbmdCb3giIGlkPSJmaWx0ZXItMiI+CiAgICAgICAgICAgIDxmZU9mZnNldCBkeD0iMCIgZHk9IjIiIGluPSJTb3VyY2VBbHBoYSIgcmVzdWx0PSJzaGFkb3dPZmZzZXRPdXRlcjEiPjwvZmVPZmZzZXQ+CiAgICAgICAgICAgIDxmZUdhdXNzaWFuQmx1ciBzdGREZXZpYXRpb249IjIiIGluPSJzaGFkb3dPZmZzZXRPdXRlcjEiIHJlc3VsdD0ic2hhZG93Qmx1ck91dGVyMSI+PC9mZUdhdXNzaWFuQmx1cj4KICAgICAgICAgICAgPGZlQ29tcG9zaXRlIGluPSJzaGFkb3dCbHVyT3V0ZXIxIiBpbjI9IlNvdXJjZUFscGhhIiBvcGVyYXRvcj0ib3V0IiByZXN1bHQ9InNoYWRvd0JsdXJPdXRlcjEiPjwvZmVDb21wb3NpdGU+CiAgICAgICAgICAgIDxmZUNvbG9yTWF0cml4IHZhbHVlcz0iMCAwIDAgMCAwICAgMCAwIDAgMCAwICAgMCAwIDAgMCAwICAwIDAgMCAwLjA0IDAiIHR5cGU9Im1hdHJpeCIgaW49InNoYWRvd0JsdXJPdXRlcjEiPjwvZmVDb2xvck1hdHJpeD4KICAgICAgICA8L2ZpbHRlcj4KICAgIDwvZGVmcz4KICAgIDxnIGlkPSJQYWdlLTEiIHN0cm9rZT0ibm9uZSIgc3Ryb2tlLXdpZHRoPSIxIiBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPgogICAgICAgIDxnIGlkPSLln7rnoYDmtYHnqIvlm74tMDEiIHRyYW5zZm9ybT0idHJhbnNsYXRlKC02LjAwMDAwMCwgLTEwNS4wMDAwMDApIj4KICAgICAgICAgICAgPGcgaWQ9Ikdyb3VwIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxMC4wMDAwMDAsIDEwNy4wMDAwMDApIj4KICAgICAgICAgICAgICAgIDxnIGlkPSJSZWN0YW5nbGUtMTUtQ29weSI+CiAgICAgICAgICAgICAgICAgICAgPHVzZSBmaWxsPSJibGFjayIgZmlsbC1vcGFjaXR5PSIxIiBmaWx0ZXI9InVybCgjZmlsdGVyLTIpIiB4bGluazpocmVmPSIjcGF0aC0xIj48L3VzZT4KICAgICAgICAgICAgICAgICAgICA8dXNlIGZpbGwtb3BhY2l0eT0iMC45MiIgZmlsbD0iI0U2RjdGRiIgZmlsbC1ydWxlPSJldmVub2RkIiB4bGluazpocmVmPSIjcGF0aC0xIj48L3VzZT4KICAgICAgICAgICAgICAgICAgICA8cmVjdCBzdHJva2U9IiMxODkwRkYiIHN0cm9rZS13aWR0aD0iMSIgeD0iMC41IiB5PSIwLjUiIHdpZHRoPSI3OSIgaGVpZ2h0PSI0NyIgcng9IjQiPjwvcmVjdD4KICAgICAgICAgICAgICAgIDwvZz4KICAgICAgICAgICAgICAgIDx0ZXh0IGlkPSLlrqHmibnoioLngrkiIGZvbnQtZmFtaWx5PSJQaW5nRmFuZ1NDLVJlZ3VsYXIsIFBpbmdGYW5nIFNDIiBmb250LXNpemU9IjEyIiBmb250LXdlaWdodD0ibm9ybWFsIiBsaW5lLXNwYWNpbmc9IjEyIiBmaWxsPSIjMDAwMDAwIiBmaWxsLW9wYWNpdHk9IjAuNjUiPgogICAgICAgICAgICAgICAgICAgIDx0c3BhbiB4PSIxNiIgeT0iMjkiPuWuoeaJueiKgueCuTwvdHNwYW4+CiAgICAgICAgICAgICAgICA8L3RleHQ+CiAgICAgICAgICAgIDwvZz4KICAgICAgICA8L2c+CiAgICA8L2c+Cjwvc3ZnPg=="
              data-type="node"
              data-shape="rect"
              data-size="80*48"
              data-color="#1890FF"
              data-label="审批节点"
              @mousedown="addNodeToGraph"
            />
            <img
              draggable="false"
              src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iODZweCIgaGVpZ2h0PSI3OHB4IiB2aWV3Qm94PSIwIDAgODYgNzgiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+CiAgICA8IS0tIEdlbmVyYXRvcjogU2tldGNoIDQ5LjEgKDUxMTQ3KSAtIGh0dHA6Ly93d3cuYm9oZW1pYW5jb2RpbmcuY29tL3NrZXRjaCAtLT4KICAgIDx0aXRsZT5Hcm91cCAzPC90aXRsZT4KICAgIDxkZXNjPkNyZWF0ZWQgd2l0aCBTa2V0Y2guPC9kZXNjPgogICAgPGRlZnM+CiAgICAgICAgPHBhdGggZD0iTTQyLjY3MDM3MjIsMS42Njk5NTcyOCBMNzcuNjM1MzAyNiwzMy4wMjE4OTQ1IEM3OS4yODAwNjQ4LDM0LjQ5NjcwMDMgNzkuNDE3ODQxNywzNy4wMjU2MDk5IDc3Ljk0MzAzNTksMzguNjcwMzcyMiBDNzcuODQ2MTE4NSwzOC43Nzg0NTgzIDc3Ljc0MzM4ODcsMzguODgxMTg4MSA3Ny42MzUzMDI2LDM4Ljk3ODEwNTUgTDQyLjY3MDM3MjIsNzAuMzMwMDQyNyBDNDEuMTUwODI4OSw3MS42OTI1Njg3IDM4Ljg0OTE3MTEsNzEuNjkyNTY4NyAzNy4zMjk2Mjc4LDcwLjMzMDA0MjcgTDIuMzY0Njk3NDQsMzguOTc4MTA1NSBDMC43MTk5MzUxODMsMzcuNTAzMjk5NyAwLjU4MjE1ODI3MiwzNC45NzQzOTAxIDIuMDU2OTY0MSwzMy4zMjk2Mjc4IEMyLjE1Mzg4MTUsMzMuMjIxNTQxNyAyLjI1NjYxMTI5LDMzLjExODgxMTkgMi4zNjQ2OTc0NCwzMy4wMjE4OTQ1IEwzNy4zMjk2Mjc4LDEuNjY5OTU3MjggQzM4Ljg0OTE3MTEsMC4zMDc0MzEzMDMgNDEuMTUwODI4OSwwLjMwNzQzMTMwMyA0Mi42NzAzNzIyLDEuNjY5OTU3MjggWiIgaWQ9InBhdGgtMSI+PC9wYXRoPgogICAgICAgIDxmaWx0ZXIgeD0iLTguOCUiIHk9Ii02LjklIiB3aWR0aD0iMTE3LjUlIiBoZWlnaHQ9IjExOS40JSIgZmlsdGVyVW5pdHM9Im9iamVjdEJvdW5kaW5nQm94IiBpZD0iZmlsdGVyLTIiPgogICAgICAgICAgICA8ZmVPZmZzZXQgZHg9IjAiIGR5PSIyIiBpbj0iU291cmNlQWxwaGEiIHJlc3VsdD0ic2hhZG93T2Zmc2V0T3V0ZXIxIj48L2ZlT2Zmc2V0PgogICAgICAgICAgICA8ZmVHYXVzc2lhbkJsdXIgc3RkRGV2aWF0aW9uPSIyIiBpbj0ic2hhZG93T2Zmc2V0T3V0ZXIxIiByZXN1bHQ9InNoYWRvd0JsdXJPdXRlcjEiPjwvZmVHYXVzc2lhbkJsdXI+CiAgICAgICAgICAgIDxmZUNvbXBvc2l0ZSBpbj0ic2hhZG93Qmx1ck91dGVyMSIgaW4yPSJTb3VyY2VBbHBoYSIgb3BlcmF0b3I9Im91dCIgcmVzdWx0PSJzaGFkb3dCbHVyT3V0ZXIxIj48L2ZlQ29tcG9zaXRlPgogICAgICAgICAgICA8ZmVDb2xvck1hdHJpeCB2YWx1ZXM9IjAgMCAwIDAgMCAgIDAgMCAwIDAgMCAgIDAgMCAwIDAgMCAgMCAwIDAgMC4wNCAwIiB0eXBlPSJtYXRyaXgiIGluPSJzaGFkb3dCbHVyT3V0ZXIxIj48L2ZlQ29sb3JNYXRyaXg+CiAgICAgICAgPC9maWx0ZXI+CiAgICA8L2RlZnM+CiAgICA8ZyBpZD0iUGFnZS0xIiBzdHJva2U9Im5vbmUiIHN0cm9rZS13aWR0aD0iMSIgZmlsbD0ibm9uZSIgZmlsbC1ydWxlPSJldmVub2RkIj4KICAgICAgICA8ZyBpZD0i5Z+656GA5rWB56iL5Zu+LTAxIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtNy4wMDAwMDAsIC0xODQuMDAwMDAwKSI+CiAgICAgICAgICAgIDxnIGlkPSJHcm91cC0zIiB0cmFuc2Zvcm09InRyYW5zbGF0ZSgxMC4wMDAwMDAsIDE4NS4wMDAwMDApIj4KICAgICAgICAgICAgICAgIDxnIGlkPSJQb2x5Z29uIj4KICAgICAgICAgICAgICAgICAgICA8dXNlIGZpbGw9ImJsYWNrIiBmaWxsLW9wYWNpdHk9IjEiIGZpbHRlcj0idXJsKCNmaWx0ZXItMikiIHhsaW5rOmhyZWY9IiNwYXRoLTEiPjwvdXNlPgogICAgICAgICAgICAgICAgICAgIDx1c2UgZmlsbC1vcGFjaXR5PSIwLjkyIiBmaWxsPSIjRTZGRkZCIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiIHhsaW5rOmhyZWY9IiNwYXRoLTEiPjwvdXNlPgogICAgICAgICAgICAgICAgICAgIDxwYXRoIHN0cm9rZT0iIzVDREJEMyIgc3Ryb2tlLXdpZHRoPSIxIiBkPSJNNDIuMzM2NTc1NywyLjA0MjIyMDQ3IEM0MS4wMDY5NzUzLDAuODUwMDEwMjM5IDM4Ljk5MzAyNDcsMC44NTAwMTAyMzkgMzcuNjYzNDI0MywyLjA0MjIyMDQ3IEwyLjY5ODQ5Mzk2LDMzLjM5NDE1NzcgQzIuNjAzOTE4NTgsMzMuNDc4OTYwNCAyLjUxNDAzMDAyLDMzLjU2ODg0OSAyLjQyOTIyNzI5LDMzLjY2MzQyNDMgQzEuMTM4NzcyMTksMzUuMTAyNTkxMyAxLjI1OTMyNjk5LDM3LjMxNTM4NzIgMi42OTg0OTM5NiwzOC42MDU4NDIzIEwzNy42NjM0MjQzLDY5Ljk1Nzc3OTUgQzM4Ljk5MzAyNDcsNzEuMTQ5OTg5OCA0MS4wMDY5NzUzLDcxLjE0OTk4OTggNDIuMzM2NTc1Nyw2OS45NTc3Nzk1IEw3Ny4zMDE1MDYsMzguNjA1ODQyMyBDNzcuMzk2MDgxNCwzOC41MjEwMzk2IDc3LjQ4NTk3LDM4LjQzMTE1MSA3Ny41NzA3NzI3LDM4LjMzNjU3NTcgQzc4Ljg2MTIyNzgsMzYuODk3NDA4NyA3OC43NDA2NzMsMzQuNjg0NjEyOCA3Ny4zMDE1MDYsMzMuMzk0MTU3NyBMNDIuMzM2NTc1NywyLjA0MjIyMDQ3IFoiPjwvcGF0aD4KICAgICAgICAgICAgICAgIDwvZz4KICAgICAgICAgICAgICAgIDx0ZXh0IGlkPSLlpITnkIboioLngrkiIGZvbnQtZmFtaWx5PSJQaW5nRmFuZ1NDLVJlZ3VsYXIsIFBpbmdGYW5nIFNDIiBmb250LXNpemU9IjEyIiBmb250LXdlaWdodD0ibm9ybWFsIiBsaW5lLXNwYWNpbmc9IjEyIiBmaWxsPSIjMDAwMDAwIiBmaWxsLW9wYWNpdHk9IjAuNjUiPgogICAgICAgICAgICAgICAgICAgIDx0c3BhbiB4PSIxNiIgeT0iNDIiPuWkhOeQhuiKgueCuTwvdHNwYW4+CiAgICAgICAgICAgICAgICA8L3RleHQ+CiAgICAgICAgICAgIDwvZz4KICAgICAgICA8L2c+CiAgICA8L2c+Cjwvc3ZnPg=="
              data-type="node"
              data-shape="polygon"
              data-size="80*72"
              data-color="#13C2C2"
              data-label="处理节点"
              @mousedown="addNodeToGraph"
            />
            <img
              draggable="false"
              src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHN2ZyB3aWR0aD0iODhweCIgaGVpZ2h0PSI1NnB4IiB2aWV3Qm94PSIwIDAgODggNTYiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+CiAgICA8IS0tIEdlbmVyYXRvcjogU2tldGNoIDQ5LjEgKDUxMTQ3KSAtIGh0dHA6Ly93d3cuYm9oZW1pYW5jb2RpbmcuY29tL3NrZXRjaCAtLT4KICAgIDx0aXRsZT5Hcm91cCA0PC90aXRsZT4KICAgIDxkZXNjPkNyZWF0ZWQgd2l0aCBTa2V0Y2guPC9kZXNjPgogICAgPGRlZnM+CiAgICAgICAgPHJlY3QgaWQ9InBhdGgtMSIgeD0iMCIgeT0iMCIgd2lkdGg9IjgwIiBoZWlnaHQ9IjQ4IiByeD0iMjQiPjwvcmVjdD4KICAgICAgICA8ZmlsdGVyIHg9Ii04LjglIiB5PSItMTAuNCUiIHdpZHRoPSIxMTcuNSUiIGhlaWdodD0iMTI5LjIlIiBmaWx0ZXJVbml0cz0ib2JqZWN0Qm91bmRpbmdCb3giIGlkPSJmaWx0ZXItMiI+CiAgICAgICAgICAgIDxmZU9mZnNldCBkeD0iMCIgZHk9IjIiIGluPSJTb3VyY2VBbHBoYSIgcmVzdWx0PSJzaGFkb3dPZmZzZXRPdXRlcjEiPjwvZmVPZmZzZXQ+CiAgICAgICAgICAgIDxmZUdhdXNzaWFuQmx1ciBzdGREZXZpYXRpb249IjIiIGluPSJzaGFkb3dPZmZzZXRPdXRlcjEiIHJlc3VsdD0ic2hhZG93Qmx1ck91dGVyMSI+PC9mZUdhdXNzaWFuQmx1cj4KICAgICAgICAgICAgPGZlQ29tcG9zaXRlIGluPSJzaGFkb3dCbHVyT3V0ZXIxIiBpbjI9IlNvdXJjZUFscGhhIiBvcGVyYXRvcj0ib3V0IiByZXN1bHQ9InNoYWRvd0JsdXJPdXRlcjEiPjwvZmVDb21wb3NpdGU+CiAgICAgICAgICAgIDxmZUNvbG9yTWF0cml4IHZhbHVlcz0iMCAwIDAgMCAwICAgMCAwIDAgMCAwICAgMCAwIDAgMCAwICAwIDAgMCAwLjA0IDAiIHR5cGU9Im1hdHJpeCIgaW49InNoYWRvd0JsdXJPdXRlcjEiPjwvZmVDb2xvck1hdHJpeD4KICAgICAgICA8L2ZpbHRlcj4KICAgIDwvZGVmcz4KICAgIDxnIGlkPSJQYWdlLTEiIHN0cm9rZT0ibm9uZSIgc3Ryb2tlLXdpZHRoPSIxIiBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPgogICAgICAgIDxnIGlkPSLln7rnoYDmtYHnqIvlm74tMDEiIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0xMDIuMDAwMDAwLCAtMTk1LjAwMDAwMCkiPgogICAgICAgICAgICA8ZyBpZD0iR3JvdXAtNCIgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoMTA2LjAwMDAwMCwgMTk3LjAwMDAwMCkiPgogICAgICAgICAgICAgICAgPGcgaWQ9IlJlY3RhbmdsZS0xNS1Db3B5LTM1Ij4KICAgICAgICAgICAgICAgICAgICA8dXNlIGZpbGw9ImJsYWNrIiBmaWxsLW9wYWNpdHk9IjEiIGZpbHRlcj0idXJsKCNmaWx0ZXItMikiIHhsaW5rOmhyZWY9IiNwYXRoLTEiPjwvdXNlPgogICAgICAgICAgICAgICAgICAgIDx1c2UgZmlsbC1vcGFjaXR5PSIwLjkyIiBmaWxsPSIjRjlGMEZGIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiIHhsaW5rOmhyZWY9IiNwYXRoLTEiPjwvdXNlPgogICAgICAgICAgICAgICAgICAgIDxyZWN0IHN0cm9rZT0iI0IzN0ZFQiIgc3Ryb2tlLXdpZHRoPSIxIiB4PSIwLjUiIHk9IjAuNSIgd2lkdGg9Ijc5IiBoZWlnaHQ9IjQ3IiByeD0iMjMuNSI+PC9yZWN0PgogICAgICAgICAgICAgICAgPC9nPgogICAgICAgICAgICAgICAgPHRleHQgaWQ9Iue7k+adn+iKgueCuSIgZm9udC1mYW1pbHk9IlBpbmdGYW5nU0MtUmVndWxhciwgUGluZ0ZhbmcgU0MiIGZvbnQtc2l6ZT0iMTIiIGZvbnQtd2VpZ2h0PSJub3JtYWwiIGxpbmUtc3BhY2luZz0iMTIiIGZpbGw9IiMwMDAwMDAiIGZpbGwtb3BhY2l0eT0iMC42NSI+CiAgICAgICAgICAgICAgICAgICAgPHRzcGFuIHg9IjE2IiB5PSIyOSI+57uT5p2f6IqC54K5PC90c3Bhbj4KICAgICAgICAgICAgICAgIDwvdGV4dD4KICAgICAgICAgICAgPC9nPgogICAgICAgIDwvZz4KICAgIDwvZz4KPC9zdmc+"
              data-type="node"
              data-shape="ellipse"
              data-size="80*48"
              data-color="#722ED1"
              data-label="结束节点"
              @mousedown="addNodeToGraph"
            />
          </div>
        </el-aside>
        <el-main>
          <div class="process-canvas" style="height: 400px;" ref="processCanvas"></div>
        </el-main>
        <!-- 右侧配置区域 -->
        <el-aside>
          <el-tabs v-model="activeTab" type="card">
            <el-tab-pane label="流程信息" name="processInfo">
              <el-form
                :model="workflow"
                :rules="rules"
                ref="workflow"
                label-width="80px"
                style="margin-right: 8px"
              >
                <el-form-item label="名称" prop="name">
                  <el-input v-model="workflow.name" size="mini"></el-input>
                </el-form-item>
                <el-form-item label="分类" prop="projectId">
                  <el-select
                    clearable
                    filterable
                    placeholder="请选择分类"
                    size="mini"
                    style="width: 100%"
                    v-model="workflow.projectId"
                  >
                    <el-option
                      :key="index"
                      :label="option.name"
                      :value="option.id"
                      v-for="(option, index) in projectListTmp"
                    ></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="模版" prop="tpl">
                  <el-select
                    size="mini"
                    v-model="workflow.tpl"
                    multiple
                    filterable
                    clearable
                    placeholder="请选择"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in templateList"
                      :key="item.id"
                      :label="item.name"
                      :value="item.id"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="描述" prop="description">
                  <el-input type="textarea" v-model="workflow.description" size="mini"></el-input>
                </el-form-item>
              </el-form>
            </el-tab-pane>
            <el-tab-pane label="配置信息" name="cellInfo">
              <el-form
                :model="node"
                :rules="rules"
                ref="node"
                label-width="80px"
                v-if="selectCell && selectCell.shape !== 'edge'"
              >
                <el-form-item label="名称" prop="name">
                  <el-input
                    v-model="node.name"
                    size="mini"
                    @input="updateNodeName"
                    placeholder="请输入节点名称"
                  ></el-input>
                </el-form-item>
                <el-form-item label="是否隐藏" prop="isHidden" style="margin-top: -20px">
                  <el-select
                    v-model="node.isHidden"
                    placeholder="请选择是否隐藏"
                    size="mini"
                    style="width: 100%"
                    filterable
                  >
                    <el-option label="是" :value="true"></el-option>
                    <el-option label="否" :value="false"></el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="顺序" prop="orderId" style="margin-top: -20px">
                  <el-input
                    v-model.number="node.orderId"
                    size="mini"
                    placeholder="请输入顺序值"
                  ></el-input>
                </el-form-item>
                <el-form-item label="人员类型" prop="participantTypeId" style="margin-top: -20px">
                  <el-select
                    filterable
                    v-model.number="node.participantTypeId"
                    placeholder="请选择处理人类型"
                    @change="getParticipantList"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in participantTypeOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item
                  label="处理人"
                  prop="participant"
                  style="margin-top: -20px"
                  v-if="node.participantTypeId !== 0"
                >
                  <el-select
                    filterable
                    v-model="node.participant"
                    placeholder="请选择处理人"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in participantOptions"
                      :key="item.id"
                      :label="item.name === undefined ? item.nickname : item.name"
                      :value="item.id"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="分配方式" prop="distributeTypeId" style="margin-top: -20px">
                  <el-select
                    filterable
                    v-model.number="node.distributeTypeId"
                    placeholder="请选择分配方式"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in distributeTypeOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="可写模版" style="margin-top: -20px">
                  <el-select
                    filterable
                    multiple
                    v-model="node.writableTemplate"
                    placeholder="请选择可写模版"
                    size="mini"
                    style="width: 100%"
                    @change="uniqueTemplate($event, 'writable')"
                  >
                    <el-option
                      v-for="item in currentTemplateList"
                      :key="item.id"
                      :label="item.name"
                      :value="item.id"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="隐藏模版" style="margin-top: -20px">
                  <el-select
                    filterable
                    multiple
                    v-model="node.hideTemplate"
                    placeholder="请选择需要隐藏的模版"
                    size="mini"
                    style="width: 100%"
                    @change="uniqueTemplate($event, 'hidden')"
                  >
                    <el-option
                      v-for="item in currentTemplateList"
                      :key="item.id"
                      :label="item.name"
                      :value="item.id"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="状态属性" prop="attributeTypeId" style="margin-top: -20px">
                  <el-select
                    filterable
                    v-model.number="node.attributeTypeId"
                    placeholder="请选择状态属性"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in attributeTypeOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="抄送" style="margin-top: -20px">
                  <el-select
                    filterable
                    multiple
                    clearable
                    v-model.number="node.ccEmail"
                    placeholder="请选择抄送人"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in participantOptions"
                      :key="item.id"
                      :label="item.name === undefined ? item.nickname : item.name"
                      :value="item.email"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="标签" style="margin-top: -20px">
                  <el-select
                    multiple
                    clearable
                    filterable
                    v-model="node.label"
                    placeholder="请选择标签"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in labelOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
              </el-form>
              <el-form
                :model="edge"
                :rules="rules"
                ref="edge"
                label-width="80px"
                v-if="selectCell && selectCell.shape === 'edge'"
              >
                <el-form-item label="名称" prop="name">
                  <el-input
                    v-model="edge.name"
                    size="mini"
                    @input="updateEdgeName"
                    placeholder="请输入流转名称"
                  ></el-input>
                </el-form-item>
                <el-form-item
                  label="源节点"
                  prop="sourceStatus"
                  style="margin-top: -20px"
                  v-show="false"
                >
                  <el-input
                    v-model.number="edge.sourceStatus"
                    size="mini"
                    placeholder="请输入源节点"
                    readonly
                  ></el-input>
                </el-form-item>
                <el-form-item
                  label="目标节点"
                  prop="destinationStatus"
                  style="margin-top: -20px"
                  v-show="false"
                >
                  <el-input
                    v-model.number="edge.destinationStatus"
                    size="mini"
                    placeholder="请输入目标节点"
                    readonly
                  ></el-input>
                </el-form-item>
                <el-form-item label="流转属性" prop="attributeTypeId" style="margin-top: -20px">
                  <el-select
                    v-model.number="edge.attributeTypeId"
                    placeholder="请选择流转属性"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in tranAttributeTypeOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
                <el-form-item label="标签" style="margin-top: -20px">
                  <el-select
                    multiple
                    clearable
                    filterable
                    v-model="edge.label"
                    placeholder="请选择标签"
                    size="mini"
                    style="width: 100%"
                  >
                    <el-option
                      v-for="item in edgeLabelOptions"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value"
                    >
                    </el-option>
                  </el-select>
                </el-form-item>
              </el-form>
            </el-tab-pane>
          </el-tabs>
        </el-aside>
      </el-container>
    </el-container>
  </div>
</template>

<script>
// import {
//   loadProjectList, loadTemplateList, loadUserList, loadDepartList,
// } from '@/api/processCenter';
import { Graph } from '@antv/x6'
import { Export } from '@antv/x6-plugin-export'
import { Transform } from '@antv/x6-plugin-transform'
import { Snapline } from '@antv/x6-plugin-snapline'
import { Clipboard } from '@antv/x6-plugin-clipboard'
import { Keyboard } from '@antv/x6-plugin-keyboard'
import { Selection } from '@antv/x6-plugin-selection'
import { History } from '@antv/x6-plugin-history'
import { Dnd } from '@antv/x6-plugin-dnd'

export default {
  name: "ProcessEdit",
  props: {
    data: {
      type: Object,
      default: () => ({
        name: "",
        projectId: null,
        description: "",
        icon: "el-icon-user",
        iconColor: "blue",
        tpl: [],
        flowchart: {
          nodes: [],
          edges: [],
          workflow: {},
        },
      }),
    },
  },
  computed: {
    // workflow: {
    //   get () {
    //     return this.data;
    //   },
    //   set (val) {
    //     this.$emit('update:data', val);
    //   }
    // },
    currentTemplateList() {
      return this.templateList.filter((tpl) => {
        if (
          !this.workflow.tpl ||
          !Array.isArray(this.workflow.tpl) ||
          this.workflow.tpl.length === 0
        ) {
          return false;
        }
        return this.workflow.tpl.indexOf(tpl.id) >= 0;
      });
    },
  },
  watch: {
    data: {
      handler(val) {
        if (this.internalChange) {
          return;
        }
        // nodeConfigs: {},
        // edgeConfigs: {},
        this.workflow = {
          name: val.name,
          projectId: val.projectId,
          description: val.description,
          icon: val.icon,
          iconColor: val.iconColor,
          tpl: this.deepCopy(val.tpl),
          nodeConfigs: {},
          edgeConfigs: {},
        };
        const cells = [];
        if (val.flowchart.nodes.length > 0) {
          val.flowchart.nodes.forEach((node) => {
            const temp = this.deepCopy(node);
            this.workflow.nodeConfigs[node.id] = temp.valueData;
            delete temp.valueData;
            cells.push(temp);
          });
        }
        if (val.flowchart.edges.length > 0) {
          val.flowchart.edges.forEach((edge) => {
            const temp = this.deepCopy(edge);
            this.workflow.edgeConfigs[edge.id] = temp.valueData;
            delete temp.valueData;
            cells.push(temp);
          });
        }
        if (this.graph) {
          this.graph.fromJSON({ cells });
          this.graph.centerContent();
        } else {
          this.cells = cells;
        }
      },
      deep: true,
      immediate: true,
    },
  },
  data: () => ({
    cells: [],
    workflow: {},
    internalChange: false,
    graph: null,
    dnd: null,
    selectCell: null,
    activeTab: "processInfo",
    projectListTmp: [],
    templateList: [],
    node: {},
    edge: {},
    rules: {
      name: [{ required: true, message: " ", trigger: "change" }],
      tpl: [{ required: true, message: " ", trigger: "change" }],
      icon: [{ required: true, message: " ", trigger: "change" }],
      orderId: [{ required: true, message: " ", trigger: "change" }],
      participant: [{ required: true, message: " ", trigger: "change" }],
      participantTypeId: [{ required: true, message: " ", trigger: "change" }],
      distributeTypeId: [{ required: true, message: " ", trigger: "change" }],
      attributeTypeId: [{ required: true, message: " ", trigger: "change" }],
      sourceStatus: [{ required: true, message: " ", trigger: "change" }],
      projectId: [{ required: true, message: "请选择项目", trigger: "change" }],
      destinationStatus: [{ required: true, message: " ", trigger: "change" }],
    },
    participantTypeOptions: [
      { label: "无处理人", value: 0 },
      { label: "个人", value: 1 },
      { label: "业务组", value: 2 },
      // { label: '部门', value: 3 },
      { label: "变量", value: 4 },
    ],
    distributeTypeOptions: [
      // { label: '主动接单', value: 1 },
      { label: "直接处理", value: 2 },
      // { label: '随机分配', value: 3 },
      // { label: '全部处理', value: 4 }
    ],
    attributeTypeOptions: [
      { label: "新建", value: 1 },
      { label: "审批", value: 2 },
      { label: "处理", value: 3 },
      { label: "结束", value: 4 },
      { label: "其他", value: 5 },
    ],
    labelOptions: [
      { label: "抄送HRBP", value: "ccHrbp" },
      { label: "指定交接人", value: "handover" },
      { label: "分管负责人", value: "chargeLeader" },
    ],
    edgeLabelOptions: [
      { label: "leader申请", value: "leaderApply" },
      { label: "指定分管领导", value: "plusSign" },
    ],
    tranAttributeTypeOptions: [
      { label: "同意", value: 1 },
      { label: "拒绝", value: 2 },
      { label: "其他", value: 3 },
    ],
    participantOptions: [],
    participantMap: {},
    clickCellMap: {},
    loadUserPromise: [],
  }),
  created() {
    this.loadProject();
    this.loadTemplate();
    // this.loadUserListPartial();
  },
  methods: {
    uniqueTemplate(select, type) {
      // debugger;
      let uniqueList = [];
      if (type === "writable") {
        uniqueList = this.node.hideTemplate;
      } else {
        uniqueList = this.node.writableTemplate;
      }
      select.forEach((v) => {
        const index = uniqueList.findIndex((value) => value === v);
        if (index >= 0) {
          uniqueList.splice(index, 1);
        }
      });
      // this.node.name = this.node.hideTemplate.length;
      this.$forceUpdate();
    },
    async loadUserListPartialFunc() {
      const params = {
        page: 1,
        perPage: 100,
        sort: 1,
      };

      // const loadAction = async (params, res) => {
      //   return await loadUserList(params).then(response => {
      //     res.push(...response.data);
      //     return response.data.length === params.perPage;
      //   }, () => false);
      // }
      const res = [];
      let flag = await loadAction(params, res);
      while (flag) {
        params.page = params.page + 1;
        flag = await loadAction(params, res);
      }
      this.participantMap.user = res;
      while (this.loadUserPromise.length > 0) {
        const callback = this.loadUserPromise.shift();
        callback[0]();
      }
    },
    loadUserListPartial() {
      if (this.loadUserPromise.length === 0) {
        this.loadUserListPartialFunc();
      }
      return new Promise((resolve, reject) => {
        this.loadUserPromise.push([resolve, reject]);
      });
    },
    getParticipantList(init = false) {
      // this.node.participant = '';
      this.participantOptions = [];
      const params = {
        page: 1,
        perPage: 99999,
        sort: 1,
      };
      if (this.node.participantTypeId === 1) {
        if (!init || this.node.participant === 0) this.node.participant = "";
        this.participantOptions = this.participantMap.user;
        if (!this.participantMap.user) {
          this.loadUserListPartial().then(() => {
            this.participantOptions = this.participantMap.user;
          });
          // loadUserList(params).then(res => {
          //   this.participantMap.user = res.data;
          //   this.participantOptions = res.data;
          // }, err => {
          //   console.error(err);
          //   this.$message({
          //     type: 'error',
          //     message: err.errmsg,
          //   });
          // });
        } else {
          this.participantOptions = this.participantMap.user;
        }
      } else if (this.node.participantTypeId === 2) {
        if (!this.participantMap.depart) {
          // loadDepartList(params).then(res => {
          //   this.participantMap.depart = res.data;
          //   this.participantOptions = res.data;
          // }, err => {
          //   console.error(err);
          //   this.$message({
          //     type: 'error',
          //     message: err.errmsg,
          //   });
          // });
        } else {
          this.participantOptions = this.participantMap.depart;
        }
      } else if (this.node.participantTypeId === 4) {
        // 处理变量
        this.participantOptions = [
          { id: 1, name: "创建者" },
          { id: 2, name: "创建者Leader" },
          { id: 3, name: "汇报对象" },
          { id: 4, name: "分管负责人" },
          { id: 5, name: "HRBP" },
          { id: 6, name: "VP" },
        ];
        if (!init) this.node.participant = 1;
      } else if (this.node.participantTypeId === 0) {
        this.node.participant = 0;
      }
    },
    loadTemplate() {
      const params = {
        page: 1,
        perPage: 99999,
        sort: 1,
      };
      // loadTemplateList(params).then((res) => {
      //   this.templateList = res.data;
      // }, (err) => {
      //   this.$message({
      //     type: 'error',
      //     message: err.errmsg,
      //   });
      //   console.error(err);
      // });
    },
    loadProject() {
      const params = {
        page: 1,
        perPage: 99999,
        sort: 1,
      };
      // loadProjectList(params).then((res) => {
      //   this.projectListTmp = res.data;
      // }, (err) => {
      //   this.$message({
      //     type: 'error',
      //     message: err.errmsg,
      //   });
      //   console.error(err);
      // });
    },
    handleCellClick(cell) {
      this.activeTab = "cellInfo";
      this.selectCell = cell;
      const id = cell.id;
      if (cell.isNode()) {
        if (!this.workflow.nodeConfigs[id]) {
          let attributeTypeId = 1;
          if (cell.shape === "rect") {
            attributeTypeId = 2;
          } else if (cell.shape === "polygon") {
            attributeTypeId = 3;
          } else if (cell.shape === "ellipse") {
            attributeTypeId = 4;
          }
          // { label: '新建', value: 1 },
          // { label: '审批', value: 2 },
          // { label: '处理', value: 3 },
          // { label: '结束', value: 4 },
          // { label: '其他', value: 5 }
          this.workflow.nodeConfigs[id] = {
            tmpId: id,
            name: cell.attrs.label.text,
            isHidden: false,
            orderId: null,
            participantTypeId: 0,
            participant: "",
            distributeTypeId: 2,
            attributeTypeId,
            writableTemplate: [],
            hideTemplate: [],
            label: [],
            ccEmail: [],
          };
        }
        this.node = this.workflow.nodeConfigs[id];
      } else {
        if (!this.workflow.edgeConfigs[id]) {
          this.workflow.edgeConfigs[id] = {
            tmpId: id,
            label: [],
            name: "",
            sourceStatus: cell.source.cell,
            destinationStatus: cell.target.cell,
            attributeTypeId: "",
          };
        }
        this.edge = this.workflow.edgeConfigs[id];
      }
      this.getParticipantList(true);
    },
    updateEdgeName(name) {
      this.selectCell.setLabels(name);
    },
    updateNodeName(name) {
      this.selectCell.setAttrs({
        label: {
          text: name,
        },
      });
    },
    addNodeToGraph(event) {
      const target = event.currentTarget;
      const type = target.getAttribute("data-type");
      let node = null;
      if (type === "node") {
        const shape = target.getAttribute("data-shape");
        const size = target.getAttribute("data-size");
        const width = Number(size.split("*")[0]);
        const height = Number(size.split("*")[1]);
        // data-color="#1890FF"
        const label = target.getAttribute("data-label");
        const color = target.getAttribute("data-color");
        const rgbToHex = (r, g, b) => {
          const hex = ((r << 16) | (g << 8) | b).toString(16);
          return "#" + new Array(Math.abs(hex.length - 7)).join("0") + hex;
        };

        // hex to rgb
        const hexToRgb = (hex) => {
          const rgb = [];
          for (let i = 1; i < 7; i += 2) {
            rgb.push(parseInt("0x" + hex.slice(i, i + 2)));
          }
          return rgb;
        };

        // 计算渐变过渡色
        const gradient = (startColor, endColor, step) => {
          // 将 hex 转换为rgb
          const sColor = hexToRgb(startColor);
          const eColor = hexToRgb(endColor);

          // 计算R\G\B每一步的差值
          const rStep = (eColor[0] - sColor[0]) / step;
          const gStep = (eColor[1] - sColor[1]) / step;
          const bStep = (eColor[2] - sColor[2]) / step;

          const gradientColorArr = [];
          for (let i = 0; i < step; i++) {
            // 计算每一步的hex值
            gradientColorArr.push(
              rgbToHex(
                parseInt(rStep * i + sColor[0]),
                parseInt(gStep * i + sColor[1]),
                parseInt(bStep * i + sColor[2])
              )
            );
          }
          return gradientColorArr;
        };
        const colorList = gradient(color, "#ffffff", 10);
        const halfColor = colorList[9];
        const fillColor = colorList[6];
        const ports = {
          groups: {
            top: {
              position: "top",
              attrs: {
                circle: {
                  r: 4,
                  magnet: true,
                  stroke: "#31d0c6",
                  strokeWidth: 2,
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
                  stroke: "#31d0c6",
                  strokeWidth: 2,
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
                  stroke: "#31d0c6",
                  strokeWidth: 2,
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
                  stroke: "#31d0c6",
                  strokeWidth: 2,
                  fill: "#fff",
                },
              },
            },
          },
          items: [
            {
              id: "top",
              group: "top",
            },
            {
              id: "bottom",
              group: "bottom",
            },
            {
              id: "left",
              group: "left",
            },
            {
              id: "right",
              group: "right",
            },
          ],
        };
        const nodeInfo = {
          shape,
          width,
          height,
          attrs: {
            label: {
              "font-size": 12,
              text: label,
              fill: "black",
            },
            body: {
              stroke: fillColor,
              fill: halfColor,
              strokeWidth: 2,
            },
          },
          ports,
        };
        if (shape === "polygon") {
          nodeInfo.points = "0,10 10,0 20,10 10,20";
        }
        node = this.graph.createNode(nodeInfo);
      } else {
        node = {};
      }
      this.dnd.start(node, event);
    },
    initG6() {
      const that = this;
      this.graph = new Graph({
        container: that.$refs.processCanvas,
        autoResize: true,
        connecting: {
          snap: true, // 自动吸附连接线
          allowBlank: false, // 不允许只有一个节点的线
          allowMulti: false, // 不允许在两个节点间创建多个线
          highlight: true, // 高亮可被连接的点
        },
        grid: true,
        history: true,
        snapline: {
          enabled: true, // 对齐线
          sharp: true,
        },
        scroller: {
          enabled: true,
          pageVisible: false,
          pageBreak: false,
          pannable: true,
        },
        // mousewheel: {
        //   enabled: true,
        //   modifiers: ['ctrl', 'meta'],
        // },
      });
      // this.graph.fromJSON(data);
      this.dnd = new Dnd({
        target: this.graph,
        scaled: false,
        animation: true,
      });
      this.graph.on("edge:connected", ({ isNew, edge }) => {
        if (isNew) {
          that.handleCellClick(edge);
        }
      });
      this.graph.on("cell:mouseenter", ({ cell }) => {
        if (this.clickCellMap[cell.id]) {
          clearTimeout(this.clickCellMap[cell.id]);
          delete this.clickCellMap[cell.id];
        }
        if (cell.isNode()) {
          let offset = { x: 10, y: 10 };
          if (cell.shape === "polygon") {
            offset = { x: 20, y: 20 };
          }
          cell.addTools([
            {
              name: "boundary",
              args: {
                attrs: {
                  fill: "#7c68fc",
                  stroke: "#333",
                  "stroke-width": 1,
                  "fill-opacity": 0.2,
                },
              },
            },
            {
              name: "button-remove",
              args: {
                x: 0,
                y: 0,
                offset,
              },
            },
          ]);
        } else {
          cell.addTools(["button-remove"]);
        }
      });
      this.graph.on("cell:mouseleave", ({ cell }) => {
        cell.removeTools();
      });
      this.graph.on("cell:click", ({ cell }) => {
        that.handleCellClick(cell);
      });
      this.graph.on("node:added", ({ cell }) => {
        that.handleCellClick(cell);
      });
      if (this.cells.length > 0) {
        this.graph.fromJSON({ cells: this.cells });
        this.$nextTick(() => {
          this.graph.centerContent();
        });
        this.cells = [];
      }
    },
    async saveFlow() {
      let workflowValid;
      await this.$refs.workflow.validate((valid) => {
        workflowValid = valid;
      });
      if (!workflowValid) {
        this.activeTab = "processInfo";
        this.$message({
          type: "warning",
          message: "请完成流程信息的配置",
        });
        return;
      }
      const data = this.graph.toJSON();
      const { cells } = data;
      const workflow = {
        name: this.workflow.name,
        projectId: this.workflow.projectId,
        description: this.workflow.description,
        icon: this.workflow.icon,
        iconColor: this.workflow.iconColor,
        tpl: this.deepCopy(this.workflow.tpl),
      };
      const flowchart = {};
      flowchart.workflow = this.deepCopy(workflow);
      flowchart.nodes = [];
      flowchart.edges = [];
      const that = this;
      const checkEmpty = (v) => v === null || v === undefined || v === "";
      const checkNode = (form) => {
        if (checkEmpty(form.name)) {
          return false;
        }
        if (checkEmpty(form.orderId)) {
          return false;
        }
        if (checkEmpty(form.participantTypeId)) {
          return false;
        }
        if (checkEmpty(form.participant)) {
          return false;
        }
        if (checkEmpty(form.distributeTypeId)) {
          return false;
        }
        return !checkEmpty(form.attributeTypeId);
      };
      const checkEdge = (form) => {
        if (checkEmpty(form.name)) {
          return false;
        }
        return form.attributeTypeId;
      };
      const cellValid = cells.every((cell) => {
        // 线
        if (cell.shape === "edge") {
          const { id } = cell;
          if (!that.workflow.edgeConfigs[id]) {
            const cellObj = that.graph.getCell(id);
            that.handleCellClick(cellObj);
            that.$nextTick(() => {
              that.$refs.edge.validate();
            });
            return false;
          }
          if (!checkEdge(that.workflow.edgeConfigs[id])) {
            const cellObj = that.graph.getCell(id);
            that.handleCellClick(cellObj);
            that.$nextTick(() => {
              that.$refs.edge.validate();
            });
            this.$message({
              type: "warning",
              message: "请完成流转信息的配置",
            });
            return false;
          }
          const temp = this.deepCopy(cell);
          temp.valueData = that.workflow.edgeConfigs[id];
          flowchart.edges.push(temp);
          return true;
        } else {
          const { id } = cell;
          if (!that.workflow.nodeConfigs[id]) {
            const cellObj = that.graph.getCell(id);
            that.activeTab = "cellInfo";
            that.handleCellClick(cellObj);
            that.$nextTick(() => {
              that.$refs.node.validate();
            });
            return false;
          }
          if (!checkNode(that.workflow.nodeConfigs[id])) {
            const cellObj = that.graph.getCell(id);
            that.activeTab = "cellInfo";
            that.handleCellClick(cellObj);
            that.$nextTick(() => {
              that.$refs.node.validate();
            });
            this.$message({
              type: "warning",
              message: "请完成节点信息的配置",
            });
            return false;
          }
          const temp = this.deepCopy(cell);
          temp.valueData = that.workflow.nodeConfigs[id];
          flowchart.nodes.push(temp);
          return true;
        }
      });
      // workflow.flowchart = flowchart;
      if (!cellValid) {
        return;
      }
      this.internalChange = true;
      this.$emit("update", flowchart);
      this.$nextTick(() => {
        this.internalChange = false;
      });
    },
    deepCopy(obj, cache = []) {
      // just return if obj is immutable value
      if (obj === null || typeof obj !== "object") {
        return obj;
      }

      // if obj is hit, it is in circular structure
      const hit = find(cache, (c) => c.original === obj);
      if (hit) {
        return hit.copy;
      }

      const copy = Array.isArray(obj) ? [] : {};
      // put the copy into cache at first
      // because we want to refer it in recursive this.deepCopy
      cache.push({
        original: obj,
        copy,
      });

      Object.keys(obj).forEach((key) => {
        copy[key] = this.deepCopy(obj[key], cache);
      });

      return copy;
    },
  },
  mounted() {
    this.initG6();
  },
};
</script>

<style scoped>
.process-edit-container {
  margin-top: 20px;
  /* height: calc(100% - 20px); */
  height: 90vh;
}
.process-canvas {
  width: 100%;
  height: 100%;
}
.process-component-list {
  height: 100%;
  background-color: #e6e6e6;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-around;
  align-content: flex-start;
}
</style>
