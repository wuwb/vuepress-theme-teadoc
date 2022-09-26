<script setup lang="ts">
import NavbarBrand from '@theme/NavbarBrand.vue'
import NavbarItems from '@theme/NavbarItems.vue'
import ToggleColorModeButton from '@theme/ToggleColorModeButton.vue'
import ToggleSidebarButton from '@theme/ToggleSidebarButton.vue'
import { computed, onMounted, reactive, ref } from 'vue'
import { useThemeLocaleData } from '../composables/index.js'

defineEmits(['toggle-sidebar'])

const themeLocale = useThemeLocaleData()

const navbar = ref<HTMLElement | null>(null)
const navbarBrand = ref<HTMLElement | null>(null)
const state = reactive({ isShowShadow: false })

const linksWrapperMaxWidth = ref(1)
const linksWrapperStyle = computed(() => {
  if (!linksWrapperMaxWidth.value) {
    return {}
  }
  return {
    maxWidth: linksWrapperMaxWidth.value + 'px',
  }
})

// avoid overlapping of long title and long navbar links
onMounted(() => {
  // TODO: migrate to css var
  // refer to _variables.scss
  const MOBILE_DESKTOP_BREAKPOINT = 1048
  const navbarHorizontalPadding =
    getCssValue(navbar.value, 'paddingLeft') +
    getCssValue(navbar.value, 'paddingRight')
  const handleLinksWrapWidth = (): void => {
    if (window.innerWidth <= MOBILE_DESKTOP_BREAKPOINT) {
      linksWrapperMaxWidth.value = 0
    } else {
      linksWrapperMaxWidth.value =
        navbar.value!.offsetWidth -
        navbarHorizontalPadding -
        (navbarBrand.value?.offsetWidth || 0)
    }
  }
  handleLinksWrapWidth()
  window.addEventListener('resize', handleLinksWrapWidth, false)
  window.addEventListener('orientationchange', handleLinksWrapWidth, false)

  /**
   * 监听滚动条
   */
  const listenScroll = () => {
    const handle = (e: any) => {
      const top = document.documentElement.scrollTop || document.body.scrollTop
      if (top >= 2) {
        state.isShowShadow = true
      } else {
        state.isShowShadow = false
      }
    }
    if (!linksWrapperMaxWidth.value) {
      state.isShowShadow = true
    } else {
      window.addEventListener('scroll', handle)
      handle(null)
    }
  }

  listenScroll()
})

function getCssValue(el: HTMLElement | null, property: string): number {
  // NOTE: Known bug, will return 'auto' if style value is 'auto'
  const val = el?.ownerDocument?.defaultView?.getComputedStyle(el, null)?.[
    property
  ]
  const num = Number.parseInt(val, 10)
  return Number.isNaN(num) ? 0 : num
}
</script>

<template>
  <header ref="navbar" class="navbar" :class="{ shadow: state.isShowShadow }">
    <div class="inner">
      <ToggleSidebarButton @toggle="$emit('toggle-sidebar')" />

      <span ref="navbarBrand">
        <NavbarBrand class="home-link" />
      </span>

      <div class="navbar-items-wrapper" :style="linksWrapperStyle">
        <slot name="before" />
        <NavbarItems class="can-hide" />
        <slot name="after" />
        <ToggleColorModeButton v-if="themeLocale.colorModeSwitch" />
        <NavbarSearch v-if="!linksWrapperMaxWidth" />
      </div>
    </div>
  </header>
</template>
