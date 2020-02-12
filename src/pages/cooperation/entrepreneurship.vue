<template>
  <main class="main">
    <form class="user-from">
      <header class="user-from-header">创业经历</header>
      <ul class="user-from-list">
        <li class="user-from-list-item">
          <div class="user-from-label">{{start.title}}</div>
          <div class="user-from-input flex-right entrepreneurship-form-select" :id="start.id">
            <span class="font-light">请选择</span>
          </div>
          <span class="icon-right"></span>
        </li>
        <li class="user-from-list-item">
          <div class="user-from-label">{{end.title}}</div>
          <div class="user-from-input flex-right entrepreneurship-form-select" :id="end.id">
            <span class="font-light">{{select}}</span>
          </div>
          <span class="icon-right"></span>
        </li>

        <li class="user-from-list-item">
          <div class="user-from-label">{{type.title}}</div>
          <div class="user-from-input flex-right entrepreneurship-form-select" :id="type.id">
            <span class="font-light">{{select}}</span>
          </div>
          <span class="icon-right"></span>
        </li>
        <li class="user-from-list-item">
          <div class="user-from-label">{{capital.title}}</div>
          <div class="user-from-input flex-right entrepreneurship-form-select" :id="capital.id">
            <span class="font-light">请选择</span>
          </div>
          <span class="icon-right"></span>
        </li>
        <li class="user-from-list-item">
          <label class="user-from-label">投资金额(万元)</label>
          <input
              class="user-from-input"
              id="investment"
              type="text"
              :placeholder="placeholder"
              v-model="investment"
              @keyup="changeInvestment"
          >
        </li>
        <li class="user-from-list-item">
          <label class="user-from-label">公司名称</label>
          <input class="user-from-input" type="text" :placeholder="placeholder" v-model="name" @input="verifyString('name')">
        </li>
        <li class="user-from-list-item">
          <label class="user-from-label">经营范围</label>
          <input class="user-from-input" type="text" :placeholder="placeholder" v-model="range" @input="verifyString('range')">
        </li>
        <li class="user-from-list-item">
          <label class="user-from-label">团队人数</label>
          <input class="user-from-input" id="team" type="text" :placeholder="placeholder" v-model="team" @keyup="changeTeam">
        </li>
        <li class="user-from-list-item">
          <label class="user-from-label">年营业额(万元)</label>
          <input
              class="user-from-input"
              id="turnover"
              type="text"
              :placeholder="placeholder"
              v-model="turnover"
              @keyup="changeTurnover">
        </li>
      </ul>
    </form>
    <div class="bottom-btn-content">
      <button class="prev-button" @click="returnBtn">
        返回
      </button>
      <button class="next-button" @click="submit">
        确定
      </button>
    </div>
  </main>
</template>

<script>
  import data from '../../../static/js/data'
  function createConfig () {
    return {
      select: '请选择',
      placeholder: '请输入',
      start: {
        id: 'shipStartTime',
        title: '开始时间'
      },
      end: {
        id: 'shipEndTime',
        title: '结束时间'
      },
      type: {
        id: 'shipType',
        title: '类型'
      },
      capital: {
        id: 'shipCapital',
        title: '资金'
      },
      investment: null,
      name: null,
      range: null,
      team: null,
      turnover: null,
    }
  }

  const { datetime, entrepreneurship: { type, capital } } = data
  const datas = datetime(2019, 2019 - 40, true)

  export default {
    props: ['submitTrue', 'showEnterPage'],
    data () {
      return {
        selectPlugin: {},
        ...createConfig(),
      }
    },
    watch: {
      submitTrue(newV,oldV) {
        // do something
        if(newV) {
          this.returnBtn()
        }
        // console.log(newV,oldV)
      }
    },

    methods: {
      returnBtn() {
        const copy = createConfig()
        for(let key in copy) {
          this[key] = copy[key]
        }

        const formSelect = document.querySelectorAll('.entrepreneurship-form-select')

        for(let i = 0; i < formSelect.length; i++) {
          formSelect[i].innerHTML = '<span class="font-light">请选择</span>'
        }

        for(let key in this.selectPlugin) {
          this.selectPlugin[key].updateWheels(datas)
        }


        this.$emit('returnBtn')

      },

      initStartTime(data) {
        this.selectPlugin.select1 = new MobileSelect({
          trigger: '#' + this.start.id,
          title: this.start.title,
          wheels: [{ data }],
          position: [10, 6],
          callback: (indexArr, data) => {
            const year = data[0].value
            const day = data[1].value
            this.start = Object.assign(this.start, { value: `${year} ${day}` })

            const _date = datas.map(item => {
              const { value, childs } = item
              if(value === year) {
                return {
                  value,
                  childs: childs.map(item => item.value >= day ? item : false).filter(item => item)
                }
              }
              if(value > year) {
                return item
              }
            }).filter(item => item)

            this.selectPlugin.select2.updateWheels(_date)
          }
        });
      },

      initEndTime(data) {
        this.selectPlugin.select2 = new MobileSelect({
          trigger: '#' + this.end.id,
          title: this.end.title,
          wheels: [{ data }],
          callback: (indexArr, data) => {
            const year = data[0].value
            const day = data[1].value
            this.end = Object.assign(this.end, { value: `${year} ${day}` })

            const _date = datas.map(item => {
              const { value, childs } = item
              if(value === year) {
                return {
                  value,
                  childs: childs.map(item => item.value <= day ? item : false).filter(item => item)
                }
              }
              if(value < year) {
                return item
              }
            }).filter(item => item)

            console.log(`${year} ${day}`, _date)
            this.selectPlugin.select1.updateWheels(_date)

          }
        });
      },

      initEnterType(data) {
        new MobileSelect({
          trigger: '#' + this.type.id,
          title: this.type.title,
          wheels: [{ data }],
          callback: (indexArr, data) => {
            this.type = Object.assign(this.type, { value: data[0] })
          }
        });
      },

      initCapital(data) {
        new MobileSelect({
          trigger: '#' + this.capital.id,
          title: this.capital.title,
          wheels: [{ data }],
          callback: (indexArr, data) => {
            this.capital = Object.assign(this.capital, { value: data[0] })
          }
        });
      },

      verifyString(key) {
        const value = this[key]
        // const regx = /^[\w|\u4e00-\u9fa5|_|\.|\s]+$/
        const regx = /[^\w|^\u4e00-\u9fa5|^\.|^\s]+/g
        if(value) {
          this[key] = value.replace(regx, '').replace('^', '')
        }
      },

      changeInvestment() {
        let investment = this.investment
        if(investment) {
          if(investment * 1 >= 0) {
            const value = investment.toString().match(/^\d{0,10}(?:\.\d{0,6})?/)
            this.investment = value[0]
            console.log(value)
          } else {
            this.investment = ''
          }
        }
      },
      changeTurnover() {
        let turnover = this.turnover
        if(turnover) {
          if(turnover * 1 >= 0) {
            const value = turnover.toString().match(/^\d{0,10}(?:\.\d{0,6})?/)
            this.turnover = value[0]
          } else {
            this.turnover = ''
          }
        }
      },
      changeTeam() {
        let team = this.team
        if(team) {
          if(team * 1 >= 0) {
            this.team = parseInt(team)
          } else {
            this.team = ''
          }
        }
      },
      /**
       *
       */
      compose(data) {
        const options = {}
        // console.log('data => ', data)
        if(data.start && data.start.value) {
          options.businessStartTime =  data.start.value.split(' ').join('-') + '-01 00:00:00'
        }
        if(data.end && data.end.value) {
          options.businessEndTime = data.end.value.split(' ').join('-') + '-01 00:00:00'
        }
        // 类型
        if(data.type.value) options.businessType = data.type.value.id
        // 资金
        if(data.capital.value) options.isFinancing = data.capital.value.id

        // 投资金额
        options.investmentAmount = data.investment
        // 公司名称
        options.companyName = data.name
        // 经营范围
        options.operateRange = data.range
        // 团队数量
        options.teamNum = data.team
        // 年营业额
        options.yearTurnover = data.turnover

        return options
      },

      submit() {
        let data = createConfig()
        const keys = ['businessStartTime', 'businessEndTime', 'businessType', 'isFinancing',
          'investmentAmount', 'companyName', 'operateRange', 'teamNum', 'yearTurnover']
        let ifReturn = false

        for(let key in data) {
          data[key] = this[key]
        }
        data = this.compose(data)

        for(let key of keys) {
          // console.log(data[key], key)
          if(data[key]) {
            ifReturn = true
          }
        }
        if(!ifReturn) {
          this.$emit('submitEmpty')
          return
        }

        this.$emit('submit', data)
        // sessionStorage.setItem('entrepreneurship', JSON.stringify(config));
        console.log(data)
      }
    },

    created () {
      // const data = sessionStorage.getItem('entrepreneurship', JSON.stringify(config));
    },

    mounted () {

      this.initStartTime(datas)
      this.initEndTime(datas)
      this.initEnterType(type)
      this.initCapital(capital)
      // document.getElementsByClassName('root')[0].scroll(0, 0)
    },

    beforeUpdate(prev, next) {
      // console.log(prev, next)
      // console.log(this.submitTrue)
    }
  }
</script>
