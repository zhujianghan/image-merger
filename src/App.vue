<script setup>
import { reactive, ref } from 'vue'
import HHeader from '@/components/HHeader.vue'
import RoleField from '@/components/RoleField.vue'

const canvasDom = ref(null)
const previewImage = ref('')
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

/**
 * name1    name2     name2
 * pic1-1   pic2-1    pic2-1
 * pic1-2   pic2-2    pic2-2
 *
 * */
function draw() {
  const columnWidth = 300
  const row1Height = 20
  const row2Height = 2.16 * 300
  const row3Height = 2.16 * 300

  const cols = data.roles.length
  canvasDom.value.width = columnWidth * cols + (cols + 1) * 10 // mx-10px
  canvasDom.value.height = row1Height + row2Height + row3Height + (3 + 1) * 10 // my-10px

  const ctx = canvasDom.value.getContext('2d')
  // 初始化背景, 及最外边框
  ctx.fillStyle = '#FFFFFF'
  ctx.fillRect(0, 0, canvasDom.value.width, canvasDom.value.height)
  // 初始化前景色
  ctx.fillStyle = '#000000'

  // current 位置
  let currentHeight = 10
  let currentWidth = 10
  for (let role of data.roles) {
    // top name
    ctx.font = '20px Arial'
    ctx.fillText(role.roleName, currentWidth + 5, currentHeight + 20, columnWidth)
    currentHeight += row1Height + 10

    const img = new Image()
    // middle healthCode
    img.src = role.healthCode
    ctx.strokeRect(currentWidth, currentHeight, columnWidth, row2Height)
    ctx.drawImage(img, currentWidth, currentHeight, columnWidth, row2Height)
    currentHeight += row2Height + 10

    // bottom travelCode
    img.src = role.travelCode
    ctx.strokeRect(currentWidth, currentHeight, columnWidth, row3Height)
    ctx.drawImage(img, currentWidth, currentHeight, columnWidth, row3Height)

    currentWidth += columnWidth + 10
    currentHeight = 10
  }
}

function preview() {
  draw()
  previewImage.value = canvasDom.value.toDataURL('image/png', 1.0)
}

function downloadImage() {
  draw()

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

    <div class="px-4" v-show="previewImage.length > 0">
      <img :src="previewImage" alt="preview">
    </div>

    <canvas ref="canvasDom" class="mx-auto border hidden"></canvas>
  </div>
</template>