<script lang="ts">
export default {}
</script>

<template>
  <span class="svg-icon-close">
    <svg
      width="26"
      height="26"
      viewBox="0 0 18 18"
      data-interactive="true"
      data-icon="cancel"
    >
      <path
        d="M9 7.586l3.182-3.182a.495.495 0 01.71.003l.7.7a.506.506 0 01.004.711L10.414 9l3.182 3.182a.495.495 0 01-.003.71l-.7.7a.506.506 0 01-.711.004L9 10.414l-3.182 3.182a.495.495 0 01-.71-.003l-.7-.7a.506.506 0 01-.004-.711L7.586 9 4.404 5.818a.495.495 0 01.003-.71l.7-.7a.506.506 0 01.711-.004L9 7.586z"
        fill-rule="evenodd"
      />
      <g class="adui-icon-interactive-cover">
        <path
          d="M9 7.586l3.182-3.182a.495.495 0 01.71.003l.7.7a.506.506 0 01.004.711L10.414 9l3.182 3.182a.495.495 0 01-.003.71l-.7.7a.506.506 0 01-.711.004L9 10.414l-3.182 3.182a.495.495 0 01-.71-.003l-.7-.7a.506.506 0 01-.004-.711L7.586 9 4.404 5.818a.495.495 0 01.003-.71l.7-.7a.506.506 0 01.711-.004L9 7.586z"
          fill-rule="evenodd"
        />
      </g>
    </svg>
  </span>
</template>

<style lang="scss" scoped>
.svg-icon-close {
  line-height: initial;
  cursor: pointer;
  svg {
    height: 26px;
    width: 26px;
    fill: #a3a3a3;
    transition: fill 0.4s;
    &:hover {
      fill: #333;
    }
  }
}
</style>
