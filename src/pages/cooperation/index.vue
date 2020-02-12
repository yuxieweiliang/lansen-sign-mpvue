<template>
  <main class="root" id="root">
    <!--    用户信息    -->
    <form class="user-from">
      <van-cell-group title="个人信息">

        <van-field
          required
          label="用户名"
          placeholder="请输入"
          input-align="right"
          :value="data.userName"
          :error="error.userName"
          @input="({mp}) => { data.userName = mp.detail }"
          @change="onUsernameChange"
          @blur="onUsernameBlur"
        />

        <LsRadioGroup
          title="性别"
          :data="dataSource.gander"
          :defaultValue="data.gander"
          @change="onGanderChange"
        />

        <van-cell
          title="出生日期"
          is-link
          :value="data.birthday"
          @click="onBirthdayClick"
        />

        <van-field
          required
          key="sssss"
          label="手机"
          placeholder="请输入"
          input-align="right"
          maxlength="11"
          type="number"
          :value="data.mobile"
          :error="error.mobile"
          @focus="() => { error.mobile = null }"
          @input="({mp}) => { data.mobile = mp.detail }"
          @change="onMobileChange"
          @blur="onMobileBlur"
        />

        <van-field
          label="微信号"
          placeholder="请输入"
          input-align="right"
          maxlength="19"
          :value="data.wechatNo"
          :error="error.wechatNo"
          @focus="() => { error.wechatNo = null }"
          @input="({mp}) => { data.wechatNo = mp.detail }"
          @change="onWeChatNoChange"
          @blur="onWeChatNoBlur"
        />

        <van-field
          required
          label="目前职业"
          placeholder="请输入"
          input-align="right"
          maxlength="11"
          :value="data.job"
          :error="error.job"
          @focus="() => { error.job = null }"
          @input="({mp}) => { data.job = mp.detail }"
          @change="onJobChange"
          @blur="onJobBlur"
        />

        <van-cell
          title="学历"
          is-link
          :value="data.education || '请添加'"
          @click="onEducationSelectShow"
        />

        <van-cell
          title="工作经历"
          is-link
          :value="data.visitorExperienceList.length ? '已添加' : '请添加'"
          link-type="navigateTo"
          url="/pages/dashboard/index"
        />
        <van-cell
          title="创业经历"
          is-link
          :value="data.entrepreneurship ? '已添加' : '请添加'"
          link-type="navigateTo"
          url="/pages/dashboard/index"
        />
        </van-cell-group>

        <!--    资源    -->
      <van-cell-group title="资源">

        <LsRadioGroup
          title="本地设计师资源"
          :data="dataSource.hasDesigner"
          :defaultValue="data.hasDesigner"
          @change="onHasDesignerChange"
        />

        <van-cell
          v-if="ifHasDesigner"
          title="本地设计师数量（预估）"
          is-link
          :value="data.designerNum || '请添加'"
          @click="onHasDesignerNumberSelectShow"
        />

        <LsRadioGroup
          title="本地装修施工单位资源"
          :data="dataSource.hasDecoration"
          :defaultValue="data.hasDecoration"
          @change="onHasDecorationChange"
        />

        <van-cell
          v-if="ifHasWorker"
          title="本地装修施工单位数量（预估）"
          is-link
          :value="data.decorationNum || '请添加'"
          @click="onHasDecorationNumberSelectShow"
        />
      </van-cell-group>

      <!--    合作意向    -->
      <van-cell-group title="合作意向">

        <van-cell
          title="拟合作城市"
          is-link
          :value="(data.city && data.province) ? data.city + '-' + data.province : '请选择'"
          @click="onAddressTreeSelectShow"
        />

        <van-cell
          title="了解朗生的渠道"
          is-link
          :value="data.findWay || '请选择'"
          @click="onFindWaySelectShow"
        />

        <van-cell
          title="拟合作方式"
          is-link
          :value="data.cooperationTypeMessage || '请选择'"
          @click="onCooperationTypeSelectShow"
        />

        <van-cell
          title="拟经营类型"
          is-link
          :value="data.operateTypeMessage || '请选择'"
          @click="onOperateTypeSelectShow"
        />

        <van-cell
          title="拟经营管理方式"
          is-link
          :value="data.managementTypeMessage || '请选择'"
          @click="onManagementTypeSelectShow"
        />

        <LsRadioGroup
          title="备选场地"
          :data="dataSource.hasSpareSite"
          :defaultValue="data.hasSpareSite"
          @change="onHasSpareSiteChange"
        />

        <van-field
          v-if="ifHasSpareSite"
          placeholder="请输入"
          label="场地面积(m²)"
          input-align="right"
          maxlength="11"
          :value="data.siteArea"
          :error="error.siteArea"
          @change="onSiteAreaChange"
        />

        <van-cell
          v-if="ifHasSpareSite"
          title="场地性质"
          is-link
          :value="data.siteNatureMessage || '请选择'"
          @click="onSiteNatureSelectShow"
        />

        <LsRadioGroup
          v-if="!ifHasSpareSite"
          title="需要协助找经营场地"
          :data="dataSource.needSite"
          :defaultValue="data.needSite"
          @change="onNeedSiteChange"
        />

        <van-cell
          v-if="isShowSiteType"
          title="场地类型"
          is-link
          :value="data.siteTypeMessage || '请选择'"
          @click="onSiteTypeSelectShow"
        />

      </van-cell-group>

    </form>

    <!--    同意协议    -->
    <div class="cooperation-container">
      <radio :checked="submit.agree" @click="onAgreeRadioClick">已阅读并同意</radio>
      <a class="cooperation-notes" :class="isChecked" @click="readFile">朗生合作须知</a>
    </div>

    <!--    提交按钮    -->
    <div class="bottom-btn-content">
      <button class="prev-button" @click="onReturnIndex">
        返回
      </button>
      <button class="next-button" @click="onSubmit">
        提交
      </button>
    </div>

    <!--    日期选择器    -->
    <van-popup
      position="bottom"
      :show="datepicker.show"
      @close="onSheetClose"
    >
      <van-datetime-picker
        type="date"
        :value="datepicker.currentDate"
        :min-date="datepicker.minDate"
        :max-date="datepicker.maxDate"
        @confirm="onBirthdayConfirm"
        @cancel="onBirthdayCancel"
      />
    </van-popup>

    <!--    上拉选择器    -->
    <van-action-sheet
      cancel-text="取消"
      :show="sheet.show"
      :actions="sheet.data"
      @close="onSheetClose"
      @select="onSheetSelect"
    />

    <!--    滚动选择器    -->
    <van-popup
      position="bottom"
      :show="select.show"
      @close="onSheetClose"
    >
      <van-picker
        show-toolbar
        :columns="select.data"
        :defaultIndex="0"
        @cancel="onSelectCancel"
        @confirm="onSelectConfirm"
      />
    </van-popup>

    <!--    滚动选择器    -->
    <van-popup
      position="bottom"
      :show="treeSelect.show"
      @close="onSheetClose"
    >
      <van-tree-select
        :items="treeSelect.data"
        :mainActiveIndex="treeSelect.mainActiveIndex"
        :activeId="treeSelect.activeId"
        :height="640"
        @clickNav="onTreeSelectNavClick"
        @clickItem="onTreeSelectItemClick"
      />
    </van-popup>
  </main>

</template>

<script>
  import LsRadioGroup from './LsRadioGroup'
  import dataSource from '../../../static/js/data'
  import address from '../../../static/js/address'
  import * as routes from '../../routes'

  const _address = address.map((item, index) => ({
    id: `${index}_${(Math.random(32)).toString(36).split('.').pop()}`,
    text: item.value,
    children: item.children.map((item, index) => ({
      id: `${index}_${(Math.random(32)).toString(36).split('.').pop()}`,
      text: item.value
    }))
  }))

  export default {
    name: 'cooperation',
    components: {
      LsRadioGroup
    },
    data () {
      return {
        submit: {
          agree: false,
          submit: false
        },
        dataSource,
        // 时间选择
        datepicker: {
          show: false,
          formatter (type, value) {
            if (type === 'year') {
              return `${value}年`
            } else if (type === 'month') {
              return `${value}月`
            }
            return value
          },
          minDate: new Date(1980, 1, 1).getTime(),
          maxDate: new Date(2010, 11, 31).getTime(),
          currentDate: new Date(2000, 1, 1).getTime()
        },

        // 上拉菜单
        sheet: {
          show: false,
          type: null, // 当前显示的类型
          data: []
        },

        // 滚动选择器列表
        select: {
          show: false,
          type: null, // 当前显示的类型
          data: []
        },

        treeSelect: {
          show: false,
          mainActiveIndex: 0,
          activeId: null,
          data: _address
        },

        // 用户信息
        data: {
          userName: null, // 用户名
          gander: 0, // 性别
          birthday: null, // 生日
          mobile: null, // 手机号
          wechatNo: null, // 微信
          job: null, // 当前职业
          education: null, // 学历

          // 工作经历
          visitorExperienceList: [], // 工作经历

          // 创业经历
          entrepreneurship: null,

          // 设计师资源
          hasDesigner: 'no',
          designerNum: null,

          // 装修项目资源
          hasDecoration: 'no',
          decorationNum: null,

          // 途径 findWay === 'other' 时 otherWay = 自定义
          findWay: null,
          otherWay: null,

          // 合作地址
          city: null,
          province: null,

          cooperationType: null, //               合作类型 { key }
          cooperationTypeMessage: null, //        合作类型 { value }
          operateType: null, //                   经营类型 { key }
          operateTypeMessage: null, //            经营类型 { value }
          managementType: null, //                经营方式 { key }
          managementTypeMessage: null, //         经营方式 { value }
          hasSpareSite: 'no', //                  备选场地 有 | 无
          siteNature: null, //                    场地性质 { key }
          siteNatureMessage: null, //             场地性质 { value }
          siteArea: null, //                      场地面积
          needSite: 'no', //                      是否需要协助
          siteType: null, //                      需要类型 { key }
          siteTypeMessage: null //                需要类型 { value }
        },
        error: {
          userName: null, // 用户名
          birthday: null, // 生日
          mobile: null, // 手机号
          job: null, // 当前职业
          education: null, // 学历

          // 合作地址
          address: null,

          cooperationType: null, // 合作类型
          operateType: null, // 经营类型
          managementType: null, // 经营方式
          siteArea: null // 场地面积
        }
      }
    },
    computed: {
      ifHasDesigner: (state) => state.data.hasDesigner === 'yes',
      ifHasWorker: (state) => state.data.hasDecoration === 'yes',
      ifHasSpareSite: (state) => state.data.hasSpareSite === 'yes',
      isShowSiteType (state) {
        console.log(state.data.needSite === 'yes' || state.data.hasSpareSite === 'yes')
        return state.data.needSite === 'yes' || state.data.hasSpareSite === 'yes'
      }
    },

    watch: { },
    methods: {
      ...routes,
      /**
       * 显示上拉选择框
       */
      onSheetShow () {
        this.sheet.show = true
      },
      /**
       * 显示上拉选择框
       */
      onAddressTreeSelectShow () {
        this.treeSelect.show = true
      },

      /**
       * 用户名
       */
      onUsernameChange ({mp}) {
        this.data.userName = mp.detail
      },
      onUsernameBlur ({target}) {
        if (!target.value) {
          this.error.userName = true
        }
      },
      /**
       * 性别
       */
      onGanderChange ({target}) {
        this.data.gander = target.value
      },

      /**
       * 生日 { 打开 }
       */
      onBirthdayClick () {
        this.datepicker.show = true
      },
      /**
       * 生日 { 确定 }
       */
      onBirthdayConfirm ({mp}) {
        this.datepicker.show = false
        this.data.birthday = mp.detail
      },
      /**
       * 生日 { 取消 }
       */
      onBirthdayCancel () {
        this.datepicker.show = false
      },

      /**
       * 手机号
       */
      onMobileChange ({mp}) {
        const pat = /^1[\d]{0,10}/g
        if (typeof mp.detail === 'string') {
          const value = mp.detail.match(pat)
          this.data.mobile = value ? value[0] : value
        }
      },
      onMobileBlur ({target}) {
        const pat = /^1[\d]{10}/g
        if (!pat.test(target.value)) {
          this.error.mobile = true
        }
      },

      /**
       * 微信号：6-19 位
       * 不验证空值 { 非必填 }
       */
      onWeChatNoChange ({mp}) {
        this.data.wechatNo = mp.detail
      },
      onWeChatNoBlur ({target}) {
        const aze = /^[a-zA-Z]([-_a-zA-Z0-9]{5,19})$/
        if (target.value && !aze.test(target.value)) {
          this.error.wechatNo = true
        }
      },

      /**
       * 目前职业
       */
      onJobChange ({target}) {
        this.data.job = target.value
      },
      onJobBlur ({target}) {
        if (!target.value) {
          this.error.job = true
        }
      },
      /**
       * 学历 { 打开 }
       */
      onEducationSelectShow () {
        this.select.show = true
        this.select.type = 'education'
        this.select.data = dataSource.education
      },

      /**
       * 设计师 {  有 | 无 }
       */
      onHasDesignerChange ({target}) {
        this.data.hasDesigner = target.value
      },
      /**
       * 设计师数量 { 打开 }
       */
      onHasDesignerNumberSelectShow () {
        this.select.show = true
        this.select.type = 'designerNum'
        this.select.data = dataSource.designerNum
      },

      /**
       * 装修单位 {  有 | 无 }
       */
      onHasDecorationChange ({target}) {
        this.data.hasDecoration = target.value
      },
      /**
       * 装修单位 { 打开 }
       */
      onHasDecorationNumberSelectShow () {
        this.select.show = true
        this.select.type = 'decorationNum'
        this.select.data = dataSource.decorationNum
      },

      /**
       * 拟合作城市 { 城市 省 }
       */
      onTreeSelectNavClick ({ target }) {
        this.treeSelect.mainActiveIndex = target.index
        this.data.city = target.text
      },

      /**
       * 拟合作城市 { 城市 市 }
       */
      onTreeSelectItemClick ({ target }) {
        this.treeSelect.activeId = target.id
        this.data.province = target.text
        this.treeSelect.show = false
      },

      /**
       * 了解渠道 { 打开 }
       */
      onFindWaySelectShow () {
        this.select.show = true
        this.select.type = 'findWay'
        this.select.data = dataSource.findWay.map(item => item.value)
      },
      /**
       * 合作方式 { 打开 }
       */
      onCooperationTypeSelectShow () {
        this.onSheetShow()
        this.sheet.type = 'cooperationType'
        this.sheet.data = [].concat(dataSource.cooperationType)
      },
      /**
       * 经营类型 { 打开 }
       */
      onOperateTypeSelectShow () {
        this.onSheetShow()
        this.sheet.type = 'operateType'
        this.sheet.data = [].concat(dataSource.operateType)
      },
      /**
       * 经营方式 { 打开 }
       */
      onManagementTypeSelectShow () {
        this.onSheetShow()
        this.sheet.type = 'managementType'
        this.sheet.data = [].concat(dataSource.managementType)
      },

      /**
       * 备选场地 {  有 | 无 }
       * @param mp
       * @param target
       */
      onHasSpareSiteChange ({mp, target}) {
        this.data.hasSpareSite = target.value
      },
      /**
       * 场地性质 { 打开 }
       */
      onSiteNatureSelectShow () {
        this.onSheetShow()
        this.sheet.type = 'siteNature'
        this.sheet.data = [].concat(dataSource.siteNature)
      },
      /**
       * 场地面积
       */
      onSiteAreaChange ({target}) {
        this.data.siteArea = target.value
      },
      /**
       * 是否需要协助 { 是 | 否 }
       * @param mp
       * @param target
       */
      onNeedSiteChange ({mp, target}) {
        this.data.needSite = target.value
      },
      /**
       * 场地类型 { 打开 }
       */
      onSiteTypeSelectShow () {
        this.onSheetShow()
        this.sheet.type = 'siteType'
        this.sheet.data = [].concat(dataSource.siteType)
      },

      /**
       * 选择框 { 关闭}
       */
      onSelectCancel () {
        this.select.show = false
        this.select.type = null
        this.select.data.length = 0
      },
      /**
       * 选择框 { 确定 }
       */
      onSelectConfirm ({target}) {
        this.data[this.select.type] = target.value
        this.onSelectCancel()
      },

      /**
       * 上拉菜单 { 关闭 }
       */
      onSheetClose () {
        this.sheet.show = false
        this.sheet.type = null
        this.sheet.data.length = 0
      },
      /**
       * 上拉菜单 { 关闭 }
       */
      onSheetSelect ({target}) {
        this.data[this.sheet.type] = target.id
        this.data[`${this.sheet.type}Message`] = target.name
        this.onSheetClose()
      },

      onAgreeRadioClick () {
        this.submit.agree = !this.submit.agree
      }
    },

    onShow () {
      console.log(this.$mp.query)
    },

    mounted () {
      console.log(JSON.parse(JSON.stringify(this.data)))
    }
  }
</script>

<style lang="less" rel="stylesheet/less">
  @import "style.less";
</style>
