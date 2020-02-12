<template>
  <main class="main">
    <section class="banner"><img src="/static/images/banner_01.png" alt=""></section>
    <!--<section class="title"><img src="/static/image/title.jpg" alt=""></section>-->
    <section class="article">
      <div class="article-title">关于朗生</div>
      <p>朗生</p>
      <p>创立于2006年</p>
      <p>总部位于深圳</p>
      <p class="empty-line">商业空间整装材料定制服务的开创者</p>

      <br/>
      <p class="justify empty-line">十多年来，深研商业空间的构建特性，基于商业空间对装饰板的个性化需求，持续创新，积累了强大的地板、墙板、家具、门、装饰板等产品的花色设计、个性定制、柔性生产与应用服务能力。</p>

      <br/>
      <p class="justify">至今，朗生已在深圳、北京、上海、广州、成都、西安、重庆、长沙、济南、太原、武汉、郑州、青岛、南京、福州、海口、沈阳、南宁、天津等50多个城市设立运营服务中心。服务客户包括万科、美的、利郎、欧派、顾家、尚品宅配、雷士等40多家上市公司及喜茶、真功夫、木北造型等1000多家商业连锁企业。</p>
    </section>
    <section class="button-box">
      <button class="default-btn" @click="routerToDetails">合作须知</button>
      <button class="success-btn" @click="routerToFrom">合作申请表</button>
    </section>
    <section class="address">
      <span class="icon"><img src="/static/images/icon-address_01.png" alt=""></span>
      <!--<span>地址：广东省深圳市龙岗区龙岗街道南联社区爱南路368号3楼东座</span>-->
      <span>广东省深圳市龙岗区爱南路368号A座2F/3F/4F</span>
    </section>
  </main>
</template>

<script>
export default {
  data () {
    return {
      time: new Date().getDate()
    }
  },

  methods: {
    routerToFrom () {
      window.location.href = './join.html'
      console.log(this)
    },
    routerToDetails () {
      window.location.href = './details.html'
      console.log(this)
    }
  },

  created () {}
}
</script>

<style lang="less">
@import "./style.less";
</style>
