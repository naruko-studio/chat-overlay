<script setup lang="ts">
const props = withDefaults(
  defineProps<{
    as?: string
    class?: string
  }>(),
  {
    as: "main",
    class: "",
  },
)
</script>

<template>
  <component
    :is="props.as"
    :class="[
      'flex w-full flex-1 flex-col gap-8 py-8',
      'px-4 sm:px-6',
      'bg-(--ui-bg) text-(--ui-text)',
      props.class,
    ]"
  >
    <slot />
  </component>
</template>
