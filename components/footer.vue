<script setup lang="ts">
import UContainer from "@nuxt/ui/components/Container.vue"
</script>

<template>
  <footer class="border-t border-(--ui-border) bg-(--ui-bg) py-6">
    <UContainer
      class="flex flex-col items-center justify-between gap-4 text-sm md:flex-row"
    >
      <div
        class="order-2 flex w-full justify-center md:order-1 md:w-auto md:justify-start"
      >
        <slot name="left" />
      </div>

      <div class="order-1 flex w-full justify-center md:order-2 md:w-auto">
        <slot />
      </div>

      <div
        class="order-3 flex w-full justify-center gap-2 md:order-3 md:w-auto md:justify-end"
      >
        <slot name="right" />
      </div>
    </UContainer>
  </footer>
</template>
