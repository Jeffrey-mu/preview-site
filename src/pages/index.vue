<script setup lang="ts">
const value = ref('')
const site_list = computed(() => {
  const list = value.value.split('\n')
  return list.map((item) => {
    const site = item.replaceAll(' ', '')
    if (site.endsWith('.com'))
      return `http://${site}`
  }).filter(Boolean)
})
</script>

<template>
  <div>
    <textarea v-model="value" type="text" autocomplete="off" name="text" class="input" w="80%" ma placeholder="输入域名" />
  </div>
  <div flex="~ wrap gap-10" justify-center>
    <MyIframe v-for="index, item in site_list" :key="item" :src="index" />
  </div>
</template>

<style>
.input {
  border: none;
  outline: none;
  border-radius: 15px;
  padding: 1em;
  background-color: #ccc;
  box-shadow: inset 2px 5px 10px rgba(0, 0, 0, 0.3);
  transition: 300ms ease-in-out;
}

.input:focus {
  background-color: white;
  transform: scale(1.05);
  box-shadow: 13px 13px 100px #969696,
    -13px -13px 100px #ffffff;
}
</style>

<route lang="yaml">
meta:
  layout: home
</route>
