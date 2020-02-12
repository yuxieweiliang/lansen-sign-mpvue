<template>
  <main class="main">

    <form class="user-from" v-for="(item, index) in work" :key="item.id">
      <header class="user-from-header">工作经历 <span class="delete-work" @click="deleteWork(index)">×</span></header>
      <ul class="user-from-list">

        <MobileSelectComponent
          :id="item.start.id + index"
          :index="index"
          :title="item.start.title"
          :wheels="item.start.wheels"
          :position="[10, 6]"
          @onSelect="onStartTimeSelect"
          @initSelect="(date) => onInitSelect1(date, index)"
        />

        <MobileSelectComponent
          :id="item.end.id + index"
          :index="index"
          :title="item.end.title"
          :wheels="item.end.wheels"
          @onSelect="onEndTimeSelect"
          @initSelect="(date) => onInitSelect2(date, index)"
        />

        <li class="user-from-list-item">
          <label class="user-from-label">行业</label>
          <input class="user-from-input" type="text" :placeholder="placeholder" v-model="item.industry" @input="verifyString('industry', index)">
        </li>
        <li class="user-from-list-item">
          <label class="user-from-label">单位</label>
          <input class="user-from-input" type="text" :placeholder="placeholder" v-model="item.company" @input="verifyString('company', index)">
        </li>
        <li class="user-from-list-item">
          <label class="user-from-label">岗位</label>
          <input class="user-from-input" type="text" :placeholder="placeholder" v-model="item.post" @input="verifyString('post', index)">
        </li>
      </ul>
    </form>

    <div class="add-new"><span class="icon-add"></span><button class="new" @click="addNewWork">添加工作经历</button></div>

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
  import MobileSelectComponent from './MobileSelect'

  const { datetime } = data
  // eslint-disable-next-line no-unused-vars
  const timer = datetime(2019, 2019 - 40, true)

  function createWorkData () {
    return {
      id: new Date().getTime(),
      start: {
        id: 'startTime',
        title: '开始时间',
        wheels: [{data: datetime(2019, 2019 - 40, true)}]
      },
      end: {
        id: 'endTime',
        title: '结束时间',
        wheels: [{data: datetime(2019, 2019 - 40, true)}]
      },
      industry: null,
      company: null,
      post: null
    }
  }
  // const work =

  function createConfig () {
    return {
      select: '请选择',
      placeholder: '请输入',
      work: [{...createWorkData()}]
    }
  }

  // const config =

  export default {
    props: ['submitTrue', 'showWorkPage'],
    data () {
      return {
        selectPlugin: {},
        ...createConfig()
      }
    },
    computed: { },
    components: {
      MobileSelectComponent
    },

    watch: {
      submitTrue (newV) {
        // do something
        if (newV) {
          this.returnBtn()
        }
      }
    },
    methods: {
      /* returnBtn() {
        const copy = createConfig()
        for(let key in copy) {
          this[key] = copy[key]
          // console.log(key, copy[key])
        }

        const formSelect = document.querySelectorAll('.form-select')

        for(let i = 0; i < formSelect.length; i++) {
          formSelect[i].innerHTML = '<span class="font-light">请选择</span>'
        }

        for(let key in this.selectPlugin) {
          this.selectPlugin[key].updateWheels(timer)
        }

        this.$emit('returnBtn')

      },
      onStartTimeSelect(data, index) {
        const _work = this.work
        const year = data[0].value
        const day = data[1].value
        _work[index].start.value = `${year} ${day}`
        this.work = _work

        const _date = timer.map(item => {
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

        this.selectPlugin[`select${index}_2`].updateWheels(_date)
      },
      onEndTimeSelect(data, index) {
        const _work = this.work
        const year = data[0].value
        const day = data[1].value
        _work[index].end.value = `${year} ${day}`
        this.work = _work

        const _date = timer.map(item => {
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

        this.selectPlugin[`select${index}_1`].updateWheels(_date)
      },

      verifyString(key, index) {
        const value = this.work[index][key]
        // const regx = /^[\w|\u4e00-\u9fa5|_|\.|\s]+$/
        const regx = /[^\w|^\u4e00-\u9fa5|^\.|^\s]+/g
        if(value) {
          this.work[index][key] = value.replace(regx, '').replace('^', '')
        }
      },

      onInitSelect1(select, index) {
        this.selectPlugin[`select${index}_1`] = select
      },
      onInitSelect2(select, index) {
        this.selectPlugin[`select${index}_2`] = select
      },
      addNewWork() {
        // console.log(work, createWorkData())
        this.work.push(createWorkData())
      },

      deleteWork(index) {
        let work = []

        for(let i = 0; i < this.work.length; i++) {
          work.push({...this.work[i]})
        }

        if(work.length > 1) {
          // work[index] = null
          // work = work.filter(i => i)
          this.work.splice(index, 1)
          // this.work = work
          console.log(this.work)
        }
      },
      /!**
       *
       *!/
      compose(data) {
        const options = {}
        // console.log('data => ', data)
        if(data.start && data.start.value) {
          options.startTime = data.start.value.split(' ').join('-') + '-01 00:00:00'
        }
        if(data.end && data.end.value) {
          options.endTime = data.end.value.split(' ').join('-') + '-01 00:00:00'
        }

        // 行业
        options.jobTrade = data.industry
        // 单位
        options.companyName = data.company
        // 岗位
        options.postName = data.post

        return options
      },

      verEmpty(data) {
        const keys = ['startTime', 'endTime', 'jobTrade', 'companyName', 'postName']
        const opt = {}
        let ifReturn = false

        for(let key of keys) {
          console.log(data[key], key)
          if(data[key]) {
            ifReturn = true
            opt[key] = data[key]
          }
        }

        if(!ifReturn) return

        return opt
      },
      submit() {
        let data = this.work.map(item => this.compose(item))
        const keys = ['startTime', 'endTime', 'jobTrade', 'companyName', 'postName']
        let ifReturn = false

        data = data.map(item => this.verEmpty(item)).filter(item => typeof item !== 'undefined')

        for(let i in data) {
          for(let key of keys) {
            if(data[i][key]) {
              ifReturn = true
            }
          }
        }
        if(!ifReturn) {
          this.$emit('submitEmpty')
          return
        }
        this.$emit('submit', data)
        // sessionStorage.setItem('work', JSON.stringify(config));
      } */
    },

    created () { },

    mounted () {
      // const datas = datetime(2019, 2019 - 40, true)
      // this.initStartTime(datas)
      // this.initEndTime(datas)
    }
  }
</script>
