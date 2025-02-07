<template>
  <div class="">
    <el-card class="box-card">
      <template #header>
        <div class="card-header">
          <span>数据集制作</span>
        </div>
      </template>
      <el-upload
        class="upload-demo"
        drag
        :action="uploadUrl"
        multiple
        :accept="acceptedFileTypes"
      >
        <el-icon class="el-icon--upload"><upload-filled /></el-icon>
        <div class="el-upload__text">
          拖拽文件到此处或者 <em>点击上传</em>
        </div>
        <template #tip>
          <div class="el-upload__tip">
            仅支持jpg、png格式的数据文件
          </div>
        </template>
      </el-upload>
      <el-form :model="form" label-width="135px" style="margin-top: 30px">
        <el-form-item label="自定义数据集名称:">
          <el-input v-model="form.name" />
        </el-form-item>
        <el-form-item label="产品交付时限:">
          <el-input-number v-model="form.days" :min="1" :max="10" controls-position="right"/>
          <span style="margin-left: 10px;color: var(--el-text-color-regular);">天</span>
        </el-form-item>
        <span class="el-form-item__label">详细信息编制/修改：<el-button type="primary" round @click="showSubTask">编辑</el-button></span>
      </el-form>
      <div style="text-align: center; margin-top: 20px">
        <el-button type="primary" round @click="submit">提交数据集</el-button>
      </div>
    </el-card>
    <el-card class="box-card"  style="margin-top: 10px" v-show="subtaskShow">
      <template #header>
        <div class="card-header">
          <span>子任务设置</span>
        </div>
      </template>
      <el-table :data="form.subtask" height="300" border style="width: 100%">
        <el-table-column width="180" >
          <!-- 自定义表头 -->
          <template #header>
            <p>
              任务编号
            </p>
          </template>
          <!-- 自定义表项/单元格内容 -->
          <template #default="scope">
            <p>{{ scope.row.id }}
              <svg-icon icon="delete"
                        style="margin-left: 10px"
                        @click="deleteRow(scope.row.id)" />
            </p>
          </template>
        </el-table-column>
        <el-table-column v-for="column in columnList" :key="column.prop">
          <!-- 自定义表头 -->
          <template #header>
            <!-- 段落：show为true -->
            <p>
              {{column.label}}
            </p>
          </template>
          <!-- 自定义表项/单元格内容 -->
          <template #default="scope">
            <!-- 双击文字或点击修改图标以更改"show"属性 -->
            <!-- scope.row为元数据，column.col为该列的'键' -->
            <p v-show="scope.row[column.prop].show" @dblclick="scope.row[column.prop].show = false">
              {{scope.row[column.prop].content}}
              <svg-icon icon="editable"  @click="scope.row[column.prop].show = false"></svg-icon>
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
      </el-table>
      <el-button class="mt-4" style="width: 100%" @click="onAddItem">增加子任务</el-button>
    </el-card>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { UploadFilled } from '@element-plus/icons-vue'
const form = ref({
  name: '',
  days: 1,
  subtask: []
})
const subtasks = ref(0)
const columnList = ref([
  { prop: 'name', label: '任务名称', show: true },
  { prop: 'type', label: '加工类型', show: true }
])
const subtaskShow = ref(false)
// 更换此处的上传接口
const uploadUrl = ref('https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15')

// 接受的文件类型
const acceptedFileTypes = ref('.jpg,.png')
const showSubTask = () => {
  subtaskShow.value = true
}
const onAddItem = () => {
  subtasks.value += 1
  let newid = ''
  if (subtasks.value < 10) {
    newid = 'WOS00' + subtasks.value
  } else if (subtasks.value < 100) {
    newid = 'WOS0' + subtasks.value
  } else {
    newid = 'WOS' + subtasks.value
  }
  form.value.subtask.push({
    id: newid,
    name: { content: 'null', show: true },
    type: { content: 'null', show: true }
  })
}
const submit = async () => {
  console.log(form.value)
}
const deleteRow = (rowid) => {
  for (let i = 0; i < form.value.subtask.length; i++) {
    if (form.value.subtask[i].id === rowid) {
      form.value.subtask.splice(i, 1)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
