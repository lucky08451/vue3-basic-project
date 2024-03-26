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

// TODO: 编辑功能
</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default>
          <el-button type="primary" link>編輯</el-button>
          <el-button type="danger" link>刪除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
