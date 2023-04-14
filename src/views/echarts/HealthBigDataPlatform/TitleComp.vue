<template>
  <div class="card-title">{{ title }}</div>
</template>

<script>
  export default {
    name: 'TitleComp',
    data() {
      return {}
    },
    props: ['title'],
    watch: {},
    methods: {},
  }
</script>

<style lang="less" scoped>
  .card-title {
    position: absolute;
    top: -11px;
    left: 50%;
    translate: -50%;
    color: #ffffff;
    font-size: 16.5px;
    font-weight: 600;
    font-style: italic;
    transform: scale(1.3, 1);
    letter-spacing: 2px;
    text-shadow: 0 0 4px #a1a1a1, 0 0 8px #010869;
  }
</style>
