<script lang="ts" setup></script>

<template>
  <div class="flex h-dvh flex-col">
    <Header />
    <Main>
      <slot />
    </Main>
    <Footer />
  </div>
</template>
