<script setup>
import { ref } from 'vue'

const props = defineProps({
  base64Image: String,
})

const emit = defineEmits([
  'update:base64Image',
])

const inputDom = ref(null)

function handleUpload() {
  const uploadedFile = inputDom.value.files[0]
  // console.log(uploadedFile)
  if (!uploadedFile.type.includes('image')) {
    inputDom.value.files = null
    alert('请上传图片')
    return
  }

  // 转 blob 显示
  const reader = new FileReader()
  reader.readAsDataURL(uploadedFile)
  reader.onloadend = function () {
    emit('update:base64Image', reader.result)
  }
}

</script>

<template>
  <div class="relative w-full h-20 bg-gray-200">
    <input
      ref="inputDom"
      class="invisible absolute left-0 top-0 w-full h-20"
      type="file"
      accept=".jpeg,.png,.jpg"
      @change="handleUpload()"
    >
    <div
      class="absolute left-0 top-0 w-full h-20 z-1 flex justify-center items-center px-4 cursor-pointer"
      @click="inputDom.click()"
    >
      <div v-show="!base64Image">+</div>
      <div v-show="base64Image">
        <img :src="base64Image" :alt="props.placeholder" class="h-20">
      </div>
    </div>

  </div>
</template>