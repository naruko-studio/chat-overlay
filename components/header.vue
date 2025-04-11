<!-- eslint-disable @typescript-eslint/no-unused-vars -->
<script setup lang="ts">
import {
  ref,
  computed,
  toRef,
  watch,
  onMounted,
  onUnmounted,
  watchEffect,
} from "vue"
import { useRoute } from "#imports"

const props = withDefaults(
  defineProps<{
    as?: string
    title?: string
    to?: string
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    toggle?: boolean | Record<string, any>
    toggleSide?: "left" | "right"
  }>(),
  {
    as: "header",
    to: "/",
    title: "Nuxt UI Header",
    toggle: true,
    toggleSide: "right",
  },
)

const open = defineModel<boolean>("open", { default: false })
const route = useRoute()

const isMobile = ref(false)
const isMounted = ref(false)

const checkIsMobile = () => {
  isMobile.value = window.innerWidth < 768
}

onMounted(() => {
  isMounted.value = true
  checkIsMobile()
  window.addEventListener("resize", checkIsMobile)
})

onUnmounted(() => {
  window.removeEventListener("resize", checkIsMobile)
})

watch(
  () => route.fullPath,
  () => {
    open.value = false
  },
)

watchEffect(() => {
  if (!isMobile.value) open.value = false
})

const ariaLabel = computed(() => props.title?.trim() || "Menu")
const Tag = computed(() => props.as || "header")
</script>

<template>
  <component
    :is="Tag"
    class="sticky top-0 z-50 w-full border-b border-(--ui-border) bg-(--ui-bg)/80 backdrop-blur dark:bg-(--ui-bg-dark)/80"
  >
    <UContainer class="flex h-16 items-center justify-between gap-4 px-4">
      <!-- Left -->
      <div class="flex items-center gap-2">
        <template v-if="toggleSide === 'left' && isMobile">
          <slot name="toggle">
            <UButton
              v-if="toggle"
              icon="i-lucide-menu"
              color="neutral"
              variant="ghost"
              @click="open = !open"
            />
          </slot>
        </template>

        <slot name="left">
          <NuxtLink :to="to" :aria-label="ariaLabel" class="text-xl font-bold">
            <slot name="title">{{ title }}</slot>
          </NuxtLink>
        </slot>
      </div>

      <!-- Center (default slot) -->
      <div class="hidden items-center gap-6 md:flex">
        <slot />
      </div>

      <!-- Right -->
      <div class="flex items-center gap-2">
        <slot name="right" />

        <template v-if="toggleSide === 'right' && isMobile">
          <slot name="toggle">
            <UButton
              v-if="toggle"
              icon="i-lucide-menu"
              color="neutral"
              variant="ghost"
              @click="open = !open"
            />
          </slot>
        </template>
      </div>
    </UContainer>
  </component>

  <!-- Slideover for mobile nav -->
  <USlideover v-if="isMounted && isMobile" v-model:open="open">
    <template #content>
      <div
        class="flex h-16 items-center justify-between border-b border-(--ui-border) px-4"
      >
        <div class="text-lg font-bold">
          <slot name="title">
            {{ title }}
          </slot>
        </div>
        <UButton
          icon="i-lucide-x"
          color="neutral"
          variant="ghost"
          @click="open = false"
        />
      </div>
      <div class="space-y-4 p-4">
        <slot name="content" />
      </div>
    </template>
  </USlideover>
</template>
