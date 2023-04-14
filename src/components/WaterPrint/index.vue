<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="mark-wrap">
    <div class="water-mark mark1">{{ waterTxt }}</div>
    <div class="water-mark mark2">{{ waterTxt }}</div>
    <div class="water-mark mark3">{{ waterTxt }}</div>
    <div class="water-mark mark4">{{ waterTxt }}</div>
    <div class="water-mark mark5">{{ waterTxt }}</div>
    <div class="water-mark mark6">{{ waterTxt }}</div>
    <div class="water-mark mark7">{{ waterTxt }}</div>
    <div class="water-mark mark8">{{ waterTxt }}</div>
    <div class="water-mark mark9">{{ waterTxt }}</div>
    <div class="water-mark mark10">{{ waterTxt }}</div>
    <div class="water-mark mark11">{{ waterTxt }}</div>
    <div class="water-mark mark12">{{ waterTxt }}</div>
    <div class="water-mark mark13">{{ waterTxt }}</div>
    <div class="water-mark mark14">{{ waterTxt }}</div>
    <div class="water-mark mark15">{{ waterTxt }}</div>
    <div class="water-mark mark16">{{ waterTxt }}</div>
  </div>
</template>

<script lang="ts" setup>
  const waterTxt = 'leichaohai8864'
</script>

<style lang="scss" scoped>
  .mark-wrap {
    display: flex;
    flex-wrap: wrap;
    position: fixed;
    z-index: 999;
  }
  .water-mark {
    user-select: none;
    font-size: 24px;
    font-family: 'MicrosoftYaHei';
    color: rgba(199, 199, 199, 0.2);
    font-weight: bold;
    z-index: 1;
    transform: rotate(-20deg);
    letter-spacing: 4px;
    width: 25%;
    text-align: center;
    height: 200px;
    line-height: 200px;
  }
</style>
