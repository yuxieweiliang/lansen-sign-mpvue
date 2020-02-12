<template>
  <main class="main">
    <van-tree-select
      :items="items"
      :mainActiveIndex="mainActiveIndex"
      :activeId="activeId"
      :height="640"
      @click-nav="onClickNav"
      @clickNav="onClickNav"
      @click-item="onClickItem"
      @clickItem="onClickItem"
    />
  </main>
</template>

<script>
  import * as routes from '../../../routes'
  import address from '../../../../static/js/address'

  export default {
    data () {
      return {
        mainActiveIndex: 0,
        activeId: null,
        items: address.map((item, index) => ({
          id: `${index}_${(Math.random(32)).toString(36).split('.').pop()}`,
          text: item.value,
          children: item.children.map((item, index) => ({
            id: `${index}_${(Math.random(32)).toString(36).split('.').pop()}`,
            text: item.value
          }))
        }))
      }
    },

    methods: {
      ...routes,
      onClickNav ({ target }) {
        this.mainActiveIndex = target.index
        this.city = target.text
      },

      onClickItem ({ target }) {
        this.activeId = target.id
        this.toCooperation({
          city: this.city,
          province: target.text
        })
      }
    },

    created () { }
  }
</script>

<style lang="less">
@import "./style.less";
</style>
