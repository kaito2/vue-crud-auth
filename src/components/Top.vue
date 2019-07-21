<template>
  <div class="city-list">
    <li v-for="(city, idx) in cities" v-bind:key="idx" v-on:click="viewCity(city.id)">
      <span class="region">{{ city.region }}</span>
      <h2>{{ city.name }}, <span>{{ city.nation }}</span></h2>
      <span class="discription">{{ city.description }}</span>
      <p class="user">{{ city.user }}さんが追加しました。</p>
    </li>
    <div class="city-view" v-if="isView">
      <div class="view-box">
        <p class="name">{{ selectedCity.name }}</p>
        <p class="area">{{ selectedCity.nation }}, {{ selectedCity.region }}</p>
        <p class="description">{{ selectedCity.description }}</p>
        <div class="btns">
          <div class="btn">edit</div>
          <div class="btn">delete</div>
        </div>
      </div>
      <div class="bg" @click="unbindCity(selectedCity.id)"></div>
    </div>
  </div>
</template>

<script>
// import firebase from 'firebase'
import { db } from '../main'
export default {
  name: 'Top',
  data () {
    return {
      cities: [],
      selectedCity: {},
      selectedCityId: '',
      isView: false
    }
  },
  firestore () {
    return {
      cities: db.collection('cities').orderBy('name')
    }
  },
  methods: {
    viewCity (id) {
      this.$bind('selectedCity', db.collection('cities').doc(id))
      this.selectedCityId = id
      this.isView = true
    },
    unbindCity (id) {
      this.isView = false
      this.$unbind('selectedCity')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
  .city-list
    display flex
    flex-wrap wrap
    justify-content center
    margin-top 50px

  li
    position relative
    list-style none
    width 28%
    margin 10px 1%
    padding 15px 10px 50px
    border 1px solid #eee
    font-size 12px

  h2
    font-size 15px

  .region
    background #555
    color #fff
    padding 5px 20px
    border-radius 20px

  .user
    position absolute
    bottom 0
    left 0
    width 100%
    background #555
    color #fff
    margin 0
    padding 5px 0

  .city-view

    .view-box
      width 60%
      padding 10px
      background #fff
      position fixed
      top 100px
      left 50%
      margin-left -30%
      z-index 10

      .name
        border 2px solid #555
        border-radius 10px
        padding 5px 15px
        margin 10px 0 0
        display inline-block

      .area
        padding-bottom 15px
        margin 10px -10px
        border-bottom 1px solid #eee

      .description
        padding 0 10%

      .btns
        display flex
        flex-wrap wrap
        justify-content center

        .btn
          width 100px
          text-align center
          background #555
          color #fff
          margin 5px
          padding 5px 0
          border-radius 5px

    .bg
      background rgba(0,0,0,.9)
      position fixed
      top 0
      left 0
      right 0
      bottom 0
</style>
