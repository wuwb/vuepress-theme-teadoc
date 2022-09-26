<script setup lang="ts">
import { onMounted, reactive, ref, toRefs } from 'vue'
import { useThemeLocaleData } from '../../composables/index.js'

const KEY = 'vuepress/teadocs/alert/elShow/'
const themeLocale = useThemeLocaleData()
const data: any = ref([])

const hide = (item) => {
  item.isShow = false
}

const close = (item) => {
  localStorage.setItem(`${KEY}${item.id}`, '0')
  hide(item)
}

onMounted(() => {
  /**
   * 解析公告数据
   */
  const paseAlert = () => {
    const array = themeLocale.value.alert || []

    array.forEach((item) => {
      let elShow: any = localStorage.getItem(`${KEY}${item.id}`)
      if (elShow === null) {
        elShow = true
      } else {
        elShow = Boolean(Number(elShow))
      }

      data.value.push(
        reactive({
          id: item.id,
          title: item.title,
          content: item.content,
          ref: `alert_${item.id}`,
          elShow,
          isShow: false,
        })
      )
    })

    showAll()
  }

  const showAll = () => {
    let index = -1
    const timer = window.setInterval(() => {
      index++
      const item = data.value[index]
      if (!item || !item.id) {
        window.clearInterval(timer)
      } else {
        if (item.elShow === true) {
          item.isShow = true
        }
      }
    }, 500)
  }

  paseAlert()
})
</script>

<template>
  <div class="alert-warp">
    <template v-for="(item, index) in data" :key="index">
      <div
        v-if="item.elShow"
        :ref="item.ref"
        class="warning custom-block teadocs-alert"
        :class="{ show: item.isShow }"
      >
        <p class="custom-block-title">{{ item.title }}</p>
        <p v-html="item.content"></p>
        <div class="btn-close" @click="close(item)">✕</div>
      </div>
    </template>
  </div>
</template>

<style lang="scss">
$MQMobile: 1048px;

.alert-warp {
  position: fixed;
  right: 0px;
  top: 60px;
  z-index: 100;

  .teadocs-alert {
    margin-bottom: 10px;
    position: relative;
    visibility: hidden;
    right: -501px;
    width: 500px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
    background-color: #fff8d2 !important;
    opacity: 0;
    transition: all 0.4s ease-in-out;

    &.show {
      right: 0px;
      opacity: 1;
      visibility: visible;
    }
    .btn-close {
      position: absolute;
      right: 20px;
      top: 10px;
      color: var(--c-text);
      font-size: 24px;
      cursor: pointer;
      transition: color 0.4s;

      &:hover {
        color: #000;
      }
    }
  }
}
@media (max-width: $MQMobile) {
  .alert-warp {
    top: var(--navbar-height);
    z-index: 0;
    width: 100%;
    .teadocs-alert {
      width: 100%;
      margin: 0px !important;
      box-sizing: border-box;
      box-shadow: none;
    }
  }
}
</style>
