<template>
  <div>
    <div class="search">
      <input type="text" class="input-search" placeholder="输入城市名或拼音" v-model="inputVal">
    </div>
    <div class="search-content" ref="search" v-show="inputVal">
      <ul>
        <li class="item border-bottom" v-for="item of list" :key="item.id" @click="handleClick(item.name)">{{item.name}}</li>
        <li class="item border-bottom" v-show="hasNoData">没有找到匹配项</li>
      </ul>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  name: 'CitySearch',
  props: {
    cities: Object
  },
  data () {
    return {
      inputVal: '',
      list: [],
      timer: null
    }
  },
  methods: {
    handleClick (city) {
      this.$store.commit('changeCity', city)
      this.$router.push('/')
    }
  },
  computed: {
    hasNoData () {
      return !this.length
    }
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.search)
  },
  watch: {
    inputVal () {
      if (this.timer) {
        clearTimeout(this.timer)
      }
      if (!this.inputVal) {
        this.list = []
        return
      }
      setTimeout(() => {
        const res = []
        for (let i in this.cities) {
          this.cities[i].forEach((value) => {
            if (value.spell.indexOf(this.inputVal) > -1 || value.name.indexOf(this.inputVal) > -1) {
              res.push(value)
            }
          })
        }
        this.list = res
      }, 100)
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '~styles/varibles.styl'
  .search
    height: .72rem
    padding: 0.2rem
    background: $bgColor
    .input-search
      box-sizing: border-box
      height: .62rem
      line-height: .62rem
      width: 100%
      text-align: center
      padding: 0.1rem
      border-radius:.1rem
      outline: none
      color: #666
  .search-content
    position: absolute
    top: 1.58rem
    left: 0
    right: 0
    bottom:0
    z-index:1
    background: #eee
    overflow: hidden
    .item
      height: .52rem
      line-height:.52rem
      padding: .2rem
      color: #666
      background: #fff
</style>
