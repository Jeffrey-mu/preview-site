<script setup lang="ts">
import { Hako } from 'vue-hako'
const props = defineProps<{
  src: string
}>()
const sizes = {
  'Default': [1280, 750],
  '1k': [1920, 1080],
  '2k': [2560, 1440],
  'Moto 4G': [360, 640],
  'Galaxy S5': [360, 640],
  'Pixel 2': [411, 731],
  'Pixel 2 XL': [411, 823],
  'iPhone 5/SE': [320, 568],
  'iPhone 6/7/8': [375, 667],
  'iPhone 6/7/8 Plus': [414, 736],
  'iPhone X': [375, 812],
  'iPad': [768, 1024],
  'iPad Pro': [1024, 1366],
  'Surface Duo': [540, 720],
  'Galaxy Fold': [280, 653],

}
const size = ref('Moto 4G')
const width = computed(() => sizes[size.value][0])
const height = computed(() => sizes[size.value][1])
const fullScreenShow = ref(false)
const reload = ref(true)

async function handle(type: string) {
  if (type === 'reload') {
    reload.value = false
    await nextTick()
    reload.value = true
  }
  if (type === 'full')
    fullScreenShow.value = true

  if (type === 'fullback')
    fullScreenShow.value = false
  if (type === 'copylink') {
    navigator.clipboard.writeText(props.src).then(() => {
    }).catch((err) => {
      console.error('无法复制文本到剪贴板：', err)
    })
  }
}

const fullScreenStyle = computed(() => {
  if (fullScreenShow.value) {
    return {
      'width': '100vw',
      'height': '100vh',
      'position': 'fixed',
      'top': 0,
      'z-index': 999,
      'background': 'white',
    }
  }
  return {}
})
</script>

<template>
  <!-- <div :style="{ width: box_size.w, height: box_size.h }" b="~ 2px #053B50"> -->
  <div b="~ 2px #053B50" :style="fullScreenStyle" overflow-hidden>
    <div class="tool" h="40px" flex="~ row-reverse gap-3" px-3 bg="#040D12">
      <div v-if="!fullScreenShow" i-carbon-center-to-fit h="40px" tool-icon-btn @click="handle('full')" />
      <div v-else i-carbon-arrow-left h="40px" tool-icon-btn @click="handle('fullback')" />
      <div i-carbon-reset h="40px" tool-icon-btn @click="handle('reload')" />
      <div i-carbon-link h="40px" tool-icon-btn />
      <div i-carbon-copy-link h="40px" tool-icon-btn @click="handle('copylink')" />
      <select id="" v-model="size" name="">
        <option v-for="item in Object.keys(sizes)" :key="item" :value="item">
          {{ item }}
        </option>
      </select>
    </div>
    <Hako h="full" w="full" :width="width" :height="height" class="window">
      <!-- <Preview shadow="lg" bg="dark:dark-700 light-100" /> -->
      <iframe v-if="reload" :src="src" frameborder="0" w="100%" style="height: 100%;" />
    </Hako>
  </div>
</template>
