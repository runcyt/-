<script setup>

import detailTable from './components/detailTable.vue';
import crowdEdit from './components/crowdEdit.vue';
import { ref, computed, toRaw, reactive, onMounted } from 'vue';
import axios from "axios"
const optionsArr = [
  {
    value: "app-header",
    label: "app头部"
  },
  {
    value: "app-banner",
    label: "首页banner"
  },
  {
    value: "message-push",
    label: "消息推送"
  },
  {
    value: "app-bottom-pointer",
    label: "底部红点"
  }
]
//map结构
const optionsMap = {

}
optionsArr.forEach((item) => {
  optionsMap[item.value] = item.label
})

//formData-值准备一个响应式数据，最后formdata发送给接口
const formData = ref([]);
const crowdRef = ref(null);
const selectChange = (value) => {
  const _arr = value.map((valueCode) => {
    let isExit = false;
    formData.value.forEach((formItem) => {
      //如果等于则代表当前这个值，已经存在于formdata，无需新建直接用当前的
      if (formItem.name === valueCode) {
        isExit = formItem;
      }
    })
    if (isExit) {
      return isExit;
    } else {
      return {
        name: valueCode,
        details: {
          duty: ""
        },
        crowd: []
      }
    }

  })
  formData.value = _arr;
  console.log(formData)
}
const selectArr = computed(() => {
  return formData.value.map((item) => {
    return item.name
  })
})
function submit(params) {
  const crowdArr = crowdRef.value.crowdArr
  formData.value.forEach((item) => {
    item.crowd = crowdArr;
  })
  axios.post("xxx", formData.value);
}

onMounted(() => {
  //回显功能测试
  setTimeout(() => {
    const res = [
      {
        "name": "app-banner",
        "details": {
          "duty": "123"
        },
        "crowd": [
          {
            "crowdName": "123阿萨德",
            "crowdSize": "111"
          }
        ]
      }, {
        "name": "app-header",
        "details": {
          "duty": "阿萨德23"
        },
        "crowd": [
          {
            "crowdName": "123阿萨德",
            "crowdSize": "111"
          }
        ]
      }
    ]
    formData.value = res
    crowdRef.value.setCrowd(res[0].crowd);

  }, 800)
})
</script>

<template>
  <div>
    {{ formData }}
    <h2>投放区域</h2>
    <el-select :model-value="selectArr" multiple @change="selectChange">
      <el-option v-for=" option  in  optionsArr " :value="option.value" :label="option.label" :key="option.value" />
    </el-select>
  </div>
  <detailTable :formData="formData" :optionsMap="optionsMap" @change="change"></detailTable>
  <crowdEdit ref="crowdRef"></crowdEdit>
  <div>
    <el-button @click="submit">提交</el-button>
  </div>
  <div>111</div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
