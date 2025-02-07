<template>
  <div class="data">
    <el-card class="box-card">
      <template #header>
        <div class="card-header">
          <span>系统服务数据查询</span>
        </div>
      </template>
      <el-table :data="servers" height="700" border style="width: 100%">
        <el-table-column v-for="column in columnList" :key="column.prop">
          <!-- 自定义表头 -->
          <template #header>
            <!-- 段落：show为true -->
            <div style="text-align: center">
              {{column.label}}
            </div>
          </template>
          <!-- 自定义表项/单元格内容 -->
          <template #default="scope">
            <!-- 双击文字或点击修改图标以更改"show"属性 -->
            <!-- scope.row为元数据，column.col为该列的'键' -->
            <p v-show="scope.row[column.prop].show" @dblclick="scope.row[column.prop].show = false">
              {{scope.row[column.prop].content}}
            </p>
            <!-- 失去焦点时更改"show"属性，显示文本 -->
            <el-input
              type="textarea"
              :autosize="{minRows:2,maxRows:4}"
              v-show="!scope.row[column.prop].show"
              v-model="scope.row[column.prop].content"
              @blur="scope.row[column.prop].show=true"
            />
          </template>
        </el-table-column>
        <el-table-column width="100">
          <!-- 自定义表头 -->
          <template #header>
            <div style="text-align: center">
              详情
            </div>
          </template>
          <!-- 自定义表项/单元格内容 -->
          <template #default="scope">
            <div style="text-align: center">
              <el-button round @click="showDetail(scope.row.id)">查看</el-button>
            </div>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
    <el-dialog v-model="dialogTableVisible" title="服务详情甘特图">
      <div style="text-align: center;">
        <el-image :src="servers[currentIndex].detail" fit="contain" />
      </div>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const servers = ref([
  {
    id: { content: 'ProdS001', show: true },
    name: { content: '轴零件S581', show: true },
    type: { content: '产品级', show: true },
    status: { content: '任务分配中', show: true },
    detail: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
  }, {
    id: { content: 'JobS001', show: true },
    name: { content: '轴加工车间', show: true },
    type: { content: '车间级', show: true },
    status: { content: '任务计算中', show: true },
    detail: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
  }, {
    id: { content: 'ProdS002', show: true },
    name: { content: '轮毂加工', show: true },
    type: { content: '产品级', show: true },
    status: { content: '任务执行中', show: true },
    detail: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
  }, {
    id: { content: 'JobS005', show: true },
    name: { content: '铣削车间', show: true },
    type: { content: '车间级', show: true },
    status: { content: '完成', show: true },
    detail: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'
  }
])
const columnList = ref([
  { prop: 'id', label: '调度服务编号', show: true },
  { prop: 'name', label: '服务名称', show: true },
  { prop: 'type', label: '服务类型', show: true },
  { prop: 'status', label: '状态', show: true }
])
const dialogTableVisible = ref(false)
const currentIndex = ref(0)
const showDetail = (rowid) => {
  for (let i = 0; i < servers.value.length; i++) {
    if (servers.value[i].id.content === rowid) {
      currentIndex.value = i
    }
  }
  dialogTableVisible.value = true
}
</script>

<style lang="scss" scoped></style>
