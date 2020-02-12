<template>
  <section class="statement">
    <div class="statement-card">
      <div class="statement-title">
        <div><span>特别声明</span></div>
      </div>
      <p class="empty-line">
        1．合作伙伴须提供真实的个人以及相关从业经历的资料，否则朗生有权终止双方的《合作意向书》/《合作协议》，若已进行相关工作或交易行为，将保留追究合作伙伴的一切法律责任（包括经济赔偿）;
      </p>
      <br/>
      <p>
        2．本合作须知未尽事宜，以正式的朗生城市运营中心《合作协议》条款为准。
      </p>
    </div>
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
  .statement {
    padding: .6rem .4rem 0;
    .rotate1 {
      top: 4rem;
      left: -1rem;
    }
    .rotate2 {
      width: 7rem;
      height: 7rem;
      top: 5rem;
      right: -2rem;
      border: .8rem solid rgba(240, 204, 161, .8);
    }
  }
  .statement-title {
    display: flex;
    justify-content: center;
    padding-bottom: .6rem;
    div {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 0 .2rem;
      position: relative;
      font-size: 18px;
      height: .8rem;
      line-height: .8rem;
      text-align: center;
      background: #d3a97b;
      transform: skew(-15deg, 0);
      span {
        display: flex;
        justify-content: center;
        position: relative;
        transform: skew(15deg, 0);
        flex-direction: column;
        align-items: center;
        &:after {
          position: absolute;
          margin-top: .6rem;
          bottom: -.2rem;
          content: '';
          display: block;
          width: 40%;
          height: 2px;
          background: #666;
        }
      }
    }
  }
  .statement-card {
    position: relative;
    margin-top: .2rem;
    width: 100%;
    line-height: .6rem;
    padding: .4rem;
    background: #fef1e1;
    border-radius: .3rem;
    box-shadow: 0 5px 0 0 #dfb98b;
    z-index: 9;
    text-align-last: left;
    text-align: justify;
  }
</style>
