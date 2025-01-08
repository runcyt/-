<script setup>
const { formData, optionsMap } = defineProps(['formData', 'optionsMap'])
</script>
<template>
    <div>
        <h2>投放详细</h2>
        <el-table :data="formData">
            <el-table-column label="区域名称">
                <template #default="scope">
                    <div>
                        {{ optionsMap[scope.row.name] }}
                    </div>
                </template>
            </el-table-column>
            <el-table-column label="投放责任人">
                <template #default="scope">
                    <el-input v-model="scope.row.details.duty" />
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>