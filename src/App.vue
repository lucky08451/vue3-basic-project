<script setup>
import Edit from "./components/Edit.vue";
import { ref, onMounted } from "vue";
import axios from "axios";

// TODO: 列表渲染
// 思路：聲明響應式list -> 調用街口獲取數據 -> 後端數據賦予list -> 綁訂到table組件
const list = ref([]);
const getList = async () => {
  // 接口調用
  const res = await axios.get("/list");
  // 將數據交給list
  list.value = res.data;
};
onMounted(() => {
  getList();
  console.log("組件載入完成");
});

// TODO: 删除功能
//思路：獲取當前行ID -> 通過id調用刪除接口 -> 刷新列表
const onDelete = async (id) => {
  await axios.delete(`/del/${id}`);
  console.log(id, "已刪除");
  getList();
};
// TODO: 编辑功能
// 回調數據( 調用詳情接口/當前行的靜態數據)
const EditRef = ref(null);
const onEdit = (row) => {
  EditRef.value.open(row);
};
// 更新

// console.log(EditRef.value);
</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{ row }">
          <el-button type="primary" @click="onEdit(row)" link>編輯</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link
            >刪除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="EditRef" @onUpdata="getList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
