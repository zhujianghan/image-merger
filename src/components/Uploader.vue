<script setup>
import { ref } from 'vue'

const props = defineProps({
  placeholder: {
    type: String,
    default: '',
  }
})
const input = ref(null)
const base64Image = ref('')

function handleUpload() {
  const uploadedFile = input.value.files[0]
  console.log(uploadedFile)
  if (!uploadedFile.type.includes('image')) {
    input.value.files = null
    alert('请上传图片')
  }

  // 转 blob 显示
  const reader = new FileReader()
  reader.readAsDataURL(uploadedFile)
  reader.onloadend = function () {
    base64Image.value = reader.result
  }
}

</script>

<template>
  <div class="relative w-full h-20 bg-gray-200">
    <input
      ref="input"
      class="invisible absolute left-0 top-0 w-full h-20"
      type="file"
      @change="handleUpload()"
    >
    <div
      class="absolute left-0 top-0 w-full h-20 z-1 flex justify-start items-center"
      @click="input.click()"
    >
      <div class="px-4" v-show="!base64Image">
        + {{ props.placeholder }}
      </div>
      <div v-show="base64Image">
        <img :src="base64Image" :alt="props.placeholder">
      </div>
    </div>

  </div>
</template>