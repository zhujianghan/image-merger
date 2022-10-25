<script setup>
import { reactive, ref } from 'vue'
import HHeader from '@/components/HHeader.vue'
import RoleField from '@/components/RoleField.vue'

const canvasDom = ref(null)
const data = reactive({
  roles: [
    {
      role: '宝名',
      roleName: '',
      healthCode: '',
      travelCode: '',
    },
    {
      role: '宝妈',
      roleName: '宝妈',
      healthCode: '',
      travelCode: '',
    },
    {
      role: '宝爸',
      roleName: '宝爸',
      healthCode: '',
      travelCode: '',
    },
  ]
})

function addMore() {
  const emptyRole = {
    role: '关系',
    roleName: '',
    healthCode: '',
    travelCode: '',
  }
  data.roles.push(emptyRole)
}

function preview() {
  const wid = 700
  canvasDom.value.width = wid

  const itemWidth = (wid - 70) / 2
  const itemHeight = 2.16 * itemWidth
  canvasDom.value.height = itemHeight * data.roles.length + (data.roles.length + 1) * 10

  const ctx = canvasDom.value.getContext('2d')
  // 初始化背景
  ctx.fillStyle = '#FFFFFF'
  ctx.fillRect(0, 0, canvasDom.value.width, canvasDom.value.height)
  // 初始化前景色
  ctx.fillStyle = '#000000'

  let currentHeight = 10
  let currentWidth = 10
  for (let role of data.roles) {
    // left name
    const arr = role.roleName.split('')
    ctx.font = '28px Arial'
    for (let index in arr) {
      ctx.fillText(arr[index], currentWidth, currentHeight + 100 + index * 36)
    }

    const img = new Image()
    // right healthCode
    img.src = role.healthCode
    ctx.strokeRect(currentWidth + 20 + 10, currentHeight, itemWidth, itemHeight)
    ctx.drawImage(img, currentWidth + 20 + 10, currentHeight, itemWidth, itemHeight)

    // right travelCode
    img.src = role.travelCode
    ctx.strokeRect(currentWidth + 20 + 10 + itemWidth + 10, currentHeight, itemWidth, itemHeight)
    ctx.drawImage(img, currentWidth + 20 + 10 + itemWidth + 10, currentHeight, itemWidth, itemHeight)

    currentWidth = 10
    currentHeight += itemHeight + 10
  }
}

function downloadImage() {
  preview()

  const el = document.createElement('a')
  el.href = canvasDom.value.toDataURL('image/png', 1.0)
  el.download = getDateFormat() + '_' + data.roles[0].roleName + '_健康宝_行程码'
  const event = new MouseEvent('click')
  el.dispatchEvent(event)
}

function getDateFormat() {
  const now = new Date()
  const yyyy = now.getFullYear()
  const mm = (now.getMonth() + 1 > 9 ? now.getMonth() + 1 : '0' + (now.getMonth() + 1))
  const dd = (now.getDate() > 10 ? now.getDate() : '0' + now.getDate())
  return '' + yyyy + mm + dd
}

</script>

<template>
  <div class="max-w-md mx-auto border">

    <div class="p-4 divide-y-2 divide-dotted divide-black">
      <HHeader/>
      <template v-for="(item,index) in data.roles" :key="index">
        <RoleField
          v-model:role="item.role"
          v-model:role-name="item.roleName"
          v-model:health-code="item.healthCode"
          v-model:travel-code="item.travelCode"
        />
      </template>

      <div class="py-10 space-y-5">
        <button
          class="py-2 rounded bg-green-600 text-white w-full"
          @click="addMore"
        >
          添加同住人
        </button>

        <button
          class="py-2 rounded bg-blue-600 text-white w-full"
          @click="preview"
        >
          预览
        </button>

        <button
          class="py-2 rounded bg-red-600 text-white w-full"
          @click="downloadImage"
        >
          下载图片
        </button>
      </div>
    </div>

    <canvas ref="canvasDom" class="mx-auto border"></canvas>
  </div>
</template>