<template>
  <div class="city-list">
    <li v-for="(city, idx) in cities" v-bind:key="idx">
      <span class="region">{{ city.region }}</span>
      <h2>{{ city.name }}, <span>{{ city.nation }}</span></h2>
      <span class="discription">{{ city.description }}</span>
      <p class="user">{{ city.user }}さんが追加しました。</p>
    </li>
  </div>
</template>

<script>
// import firebase from 'firebase'
import { db } from '../main'
export default {
  name: 'Top',
  data () {
    return {
      cities: []
    }
  },
  firestore () {
    return {
      cities: db.collection('cities').orderBy('name')
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
</style>
