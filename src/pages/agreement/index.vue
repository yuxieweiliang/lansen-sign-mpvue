<template>
  <div class="root">
    <van-tabs animated :active="active" @change="onChange">
      <van-tab title="标签 1">
        <DetailsComponent/>
      </van-tab>
      <van-tab title="标签 2">
        <InvestmentComponent/>
      </van-tab>
      <van-tab title="标签 3">
        <StatementComponent/>
      </van-tab>
    </van-tabs>
    <main class="main">
      <div class="bottom-btn-content">
        <button class="prev-button" @click="returnToIndex">
          返回
        </button>
        <button class="next-button" @click="routerToFrom">
          填写合作申请表
        </button>
      </div>
    </main>
  </div>
</template>

<script>
  import DetailsComponent from './details.vue'
  import InvestmentComponent from './investment.vue'
  import StatementComponent from './statement.vue'
  export default {
    data () {
      return {
        active: 2,
        route: [],
        page: 'details'
      }
    },

    components: {
      DetailsComponent,
      InvestmentComponent,
      StatementComponent
    },

    computed: { },

    methods: {
      onChange () {

      },
      changeToDetails () {
        this.page = 'details'
      },
      changeToInvestment () {
        this.page = 'investment'
      },
      changeToStatement () {
        this.page = 'statement'
      },
      routerToFrom () {
        window.location.href = './join.html'
        console.log(this)
      },
      returnToIndex () {
        window.location.href = './index.html'
      }
    },

    created () {

    }
  }
</script>

<style lang="less" rel="stylesheet/less">
  @import "./style.less";

</style>
