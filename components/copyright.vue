<script lang="ts" setup>
const props = defineProps<{
  year: number
  name: string
  url: string
  long?: boolean
}>()
const current = new Date().getFullYear()
const years = computed(() =>
  props.year === current ? `${current}` : `${props.year} - ${current}`,
)
</script>

<template>
  <div>
    <span v-if="props.long">
      Copyright &copy; {{ years }}
      <a :href="props.url" target="_blank">{{ props.name }}</a
      >. All rights reserved.
    </span>
    <span v-else>
      &copy; {{ years }}
      <a :href="props.url" target="_blank">{{ props.name }}</a>
    </span>
  </div>
</template>
