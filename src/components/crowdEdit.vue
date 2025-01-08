<script setup>
import { ref } from 'vue';
const crowdArr = ref([]);
function setCrowd(crowd) {
    crowdArr.value = crowd;
}
defineExpose({
    crowdArr,
    setCrowd
});
</script>
<template>
    <div>

        <h2>目标人群</h2>
        <el-table :data="crowdArr">
            <el-table-column label="人群名称">
                <template #default="scope">
                    <el-input v-model="scope.row.crowdName" />
                </template>
            </el-table-column>
            <el-table-column label="目标量级">
                <template #default="scope">
                    <el-input v-model="scope.row.crowdSize" />
                </template>
            </el-table-column>
        </el-table>
        <div>
            <el-button @click="() => {
                crowdArr.push({
                    crowdName: '',
                    crowdSize: ''
                })
            }">
                添加目标人群</el-button>
        </div>
    </div>
</template>