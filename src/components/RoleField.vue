<script setup>
import { computed, reactive, watch } from 'vue'
import Uploader from '@/components/Uploader.vue'

const props = defineProps({
  role: String,
  roleName: String,
  healthCode: String,
  travelCode: String,
})

const emit = defineEmits([
  'update:role',
  'update:roleName',
  'update:healthCode',
  'update:travelCode',
])

const data = reactive({
  healthCode: props.healthCode,
  travelCode: props.travelCode,
})

const rolePlaceholder = computed(() => {
  let ph = ''
  if (props.role === '宝名') {
    ph = '输入宝宝姓名'
  } else if (props.role === '宝爸' || props.role === '宝妈') {
    ph = ''
  } else {
    ph = '输入同住人关系, 如爷爷、奶奶、姥爷、姥姥...'
  }
  return ph
})

watch(data, () => {
  emit('update:healthCode', data.healthCode)
  emit('update:travelCode', data.travelCode)
})

</script>

<template>
  <div class="py-10 space-y-2">
    <div class="flex items-center">
      <div class="font-bold w-20 border p-2 text-center bg-gray-200 rounded-l">{{ role }}</div>
      <div class="grow">
        <input
          class="w-full border p-2 rounded-r"
          :class="{'bg-gray-200': role === '宝爸' || role === '宝妈'}"
          :placeholder="rolePlaceholder"
          :disabled="role === '宝爸' || role === '宝妈'"
          :value="roleName"
          @input="$emit('update:roleName',$event.target.value)"
        />
      </div>
    </div>

    <div class="flex justify-between">
      <div>
        <div class="border p-1 rounded-t font-bold text-center">健康宝</div>
        <div class="w-32">
          <Uploader v-model:base64-image="data.healthCode"/>
        </div>
      </div>
      <div>

        <div class="border p-1 rounded-t font-bold text-center">行程码</div>
        <div class="w-32">
          <Uploader v-model:base64-image="data.travelCode"/>
        </div>
      </div>
    </div>


  </div>
</template>