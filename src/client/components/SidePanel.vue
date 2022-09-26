<script setup lang="ts">
import Close from './global/Close.vue'
import request from '../utils/request.js'
import { reactive, onMounted, ref, toRefs } from 'vue'
import { useThemeLocaleData } from '../composables/index.js'

const themeLocale = useThemeLocaleData()

let isShow = ref(false)
let btnName = ref('')

let config = reactive({
  isShowAside: false,
  title: '访问者',
  btnName: '',
  data: {
    all: 0,
    twentyFour: 0,
  },
})

let configData = toRefs(config)

/**
 * 解析规则
 */
const parseConfig = (): any => {
  let config = themeLocale.value.sidePanel
  if (!config) return false
  if (config.title) configData.title = config.title

  if (config.btnName) {
    btnName.value = config.btnName
  }
}

/**
 * 获取访问数据
 */
const getCount = () => {
  let url = `https://analytics.numpy.org.cn/count?sourceUrl=${encodeURIComponent(
    window.location.href
  )}`
  request(
    'get',
    url,
    {},
    (data) => {
      configData.data = data
    },
    'json'
  )
}

/**
 * 初始化聊天室
 */
const initChat = () => {}

onMounted(() => {
  if (themeLocale.value.sidePanel) {
    if (window.document.documentElement.clientWidth > 760) {
      isShow.value = true
    }
    getCount()
    parseConfig()
    initChat()
  }
})
</script>

<template>
  <div class="side-panel" v-if="isShow">
    <div class="slide-warp">
      <button class="btn-slide" @click="config.isShowAside = true">
        {{ btnName }}
      </button>
    </div>
    <aside class="aside-card" :class="{ show: config.isShowAside }">
      <div class="top">
        <h1>你好，{{ config.title }}</h1>
        <div class="icon" @click="config.isShowAside = false">
          <close></close>
        </div>
      </div>
      <div class="stat">
        <div class="item">
          <div class="count">{{ config.data.all }}</div>
          <div class="label">总访问人数</div>
        </div>
        <div class="item">
          <div class="count">{{ config.data.twentyFour }}</div>
          <div class="label">24小时人数</div>
        </div>
      </div>
      <div class="chat" id="tchat" ref="chat">聊天室功能正在飞速开发中 🚀</div>
    </aside>
  </div>
</template>

<style lang="scss" scoped>
$MQMobile: 1048px;

.side-panel {
  .slide-warp {
    position: fixed;
    right: 0px;
    top: 256px;
    z-index: 1000;

    .btn-slide {
      cursor: pointer;
      background: #fff;
      text-align: center;
      padding: 10px;
      font-size: 14px;
      width: 40px;
      line-height: 1.4;
      border: none;
      margin: 0;
      box-shadow: 0 1px 20px 0 #e4e8eb;
      transition: box-shadow 0.4s;
      outline: none;

      &:hover {
        box-shadow: 0 1px 20px 10px #e4e8eb;
      }
    }
  }

  .aside-card {
    position: fixed;
    top: 0;
    right: -40vw;
    z-index: 1100;
    box-sizing: border-box;
    padding-bottom: 56px;
    max-width: 400px;
    min-width: 360px;
    width: 25vw;
    height: 100vh;
    background-color: #fff;
    box-shadow: 0 0 0 1px hsla(0, 0%, 87%, 0.35),
      0 8px 20px 0 rgba(0, 0, 0, 0.05), 0 4px 10px 0 rgba(0, 0, 0, 0.03);
    overflow: auto;
    transition: right 0.4s;

    &.show {
      right: 0px;
    }

    .top {
      position: relative;
      z-index: 1;
      padding: 40px;
      background-color: #fcfcfc;
      box-shadow: 0 1px 0 0 #f3f3f3;
      transition: all 0.35s ease;

      h1 {
        margin: 0px;
        padding: 0px;
        font-size: 28px;
        line-height: 36px;
        font-weight: 600;
      }

      .icon {
        position: absolute;
        right: 10px;
        top: 10px;
      }
    }

    .stat {
      padding: 15px;
      box-sizing: border-box;
      border-bottom: 1px solid #f3f3f3;

      &::after {
        content: '';
        clear: both;
        height: 0;
        overflow: hidden;
        visibility: hidden;
        display: block;
      }

      .item {
        float: left;
        width: calc(50% - 1px);
        text-align: center;
        border-right: 1px solid #f3f3f3;

        &:last-child {
          border: none;
        }

        .count {
          color: var(--c-text);
          font-weight: 500;
          font-size: 24px;
          line-height: 1.7;
        }

        .label {
          font-size: 14px;
        }
      }
    }

    .chat {
      height: calc(100vh - 107px);
      padding-top: 100px;
      width: 100%;
      box-sizing: border-box;
      text-align: center;
      font-style: 18px;
      color: #aeaeae;
    }
  }
}

@media (max-width: $MQMobile) {
  .side-panel {
    display: none;
  }
}
</style>
