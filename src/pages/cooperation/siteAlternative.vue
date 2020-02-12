<template>
  <main class="main">

    <form class="user-from" v-for="(item, index) in work" :key="item.id">
      <header class="user-from-header">工作经历 <span class="delete-work" @click="deleteWork(index)">×</span></header>
      <ul class="user-from-list">
        <li class="user-from-list-item" v-show="cooperation['field-backups'] === 'yes'">
          <div class="user-from-label">场地面积(m²)</div>
          <input class="user-from-input"
                 type="text"
                 id="field-area"
                 placeholder="请输入"
                 v-model="cooperation['field-area']"
                 @keyup="changeFieldArea"
          >
        </li>
        <li class="user-from-list-item" v-show="cooperation['field-backups'] === 'yes'" @click="showSelect('field-nature')">
          <div class="user-from-input">场地性质</div>
          <div class="user-from-label flex-right main-form-select" id="field-nature">
            <span class="font-light">未选择</span>
          </div>
          <span class="icon-right"></span>
        </li>
        <li class="user-from-list-item" v-show="fieldType" @click="showSelect('field-type')">
          <div class="user-from-input">场地类型</div>
          <div class="user-from-label flex-right main-form-select" id="field-type">
            <span class="font-light">未选择</span>
          </div>
          <span class="icon-right"></span>
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

  const { datetime } = data
  // eslint-disable-next-line no-unused-vars
  const timer = datetime(2019, 2019 - 40, true)

  function createWorkData () {
    return {
      id: new Date().getTime(),
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
      site: [{...createWorkData()}]
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

    watch: {
      submitTrue (newV) {
        // do something
        if (newV) {
          this.returnBtn()
        }
      }
    },
    methods: {
    /*  returnBtn() {
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

      showSelect(type) {
        // console.log('------------', type)
        select[type].show()
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
      const datas = datetime(2019, 2019 - 40, true)
      // this.initStartTime(datas)
      // this.initEndTime(datas)

    }
  }
</script>
