<script setup>
import { reactive } from 'vue'

import Uploader from '@/components/Uploader.vue'

const data = reactive({
  childName: '',
  healthCode: null,
  travelCode: null,
})
</script>

<template>
  <div class="space-y-2">
    <div class="flex items-center">
      <div class="font-bold w-20">宝名</div>
      <div class="grow">
        <input
          class="w-full border p-2 rounded"
          placeholder="宝宝姓名"
          v-model="data.childName"
        />
      </div>
    </div>

    <div class="flex">
      <div class="font-bold w-20">健康宝</div>
      <div class="grow">
        <Uploader :filedata="data.healthCode" placeholder="上传健康宝"/>
      </div>
    </div>

    <div class="flex">
      <div class="font-bold w-20">行程码</div>
      <div class="grow">
        <Uploader :filedata="data.travelCode" placeholder="上传行程码"/>
      </div>
    </div>


  </div>
</template>