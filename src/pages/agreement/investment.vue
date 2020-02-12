<template>
  <section class="investment">
    <span class="rotate1"></span>
    <span class="rotate2"></span>
    <img src="/static/images/budget.png" alt="">
  </section>
</template>

<script>
  export default {
    data () {
      return {}
    },

    methods: {},

    created () {

    }
  }
</script>

<style lang="less" rel="stylesheet/less">
  .investment {
    padding: 0;
    .rotate1 {
      top: 6rem;
      left: -1rem;
    }
    .rotate2 {
      top: 4rem;
      right: -2rem;
    }
    img {
      position: relative;
      width: 100%;
      height: 12rem;
      z-index: 10;
    }
  }
</style>
