<script setup lang="ts">
import NavbarItems from '@theme/NavbarItems.vue'
import SidebarItems from '@theme/SidebarItems.vue'
import { onMounted, ref } from 'vue'

const cursor = ref(false)
const linksWrapperMaxWidth = ref(0)

onMounted(() => {
  const bindSizeChange = () => {
    const MOBILE_DESKTOP_BREAKPOINT = 1048
    const handleLinksWrapWidth = (): void => {
      if (window.innerWidth <= MOBILE_DESKTOP_BREAKPOINT) {
        linksWrapperMaxWidth.value = 0
      } else {
        linksWrapperMaxWidth.value = document.documentElement.clientWidth
      }
    }
    handleLinksWrapWidth()
    window.addEventListener('resize', handleLinksWrapWidth, false)
    window.addEventListener('orientationchange', handleLinksWrapWidth, false)
  }
  bindSizeChange()
})
</script>

<template>
  <aside
    class="sidebar"
    :class="{ cursor: cursor === true || !linksWrapperMaxWidth }"
    @mouseenter="cursor = true"
    @mouseleave="cursor = false"
  >
    <NavbarItems />
    <slot name="top" />
    <SidebarItems />
    <slot name="bottom" />
  </aside>
</template>
