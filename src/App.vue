<template>
  <div id="app">
    <header>
      <h1>City.</h1>
      <div class="btns">
        <p class="btn" v-on:click="isAdd = true">都市を追加する</p>
      </div>
      <!-- add start -->
      <div class="user-info" v-if="isUser">
        <div class="header">
          <div class="avatar">
            <img v-if="user" :src="user.photoURL" alt="avatar">
          </div>
          <p v-if="user">{{ user.displayName }}</p>
          <p v-else>Guest</p>
        </div>
        <p class="btn" v-if="user" v-on:click="signOut">Log out</p>
        <p class="btn" v-else v-on:click="signInWithGoogle">Sign In</p>
      </div>
      <div class="sign-in" v-on:click="isUser=!isUser">
        <img v-if="user" v-bind:src="user.photoURL" alt="avatar">
        <span v-else>?</span>
      </div>
      <!-- add end -->
    </header>
    <!-- added start -->
    <div class="city-view" v-if="isAdd">
      <div class="view-box">
        <h2>都市を追加する</h2>
        <input type="text" placeholder="都市名" v-model="name">
        <input type="text" placeholder="都市の所在国" v-model="nation">
        <input type="text" placeholder="都市の所在地域" v-model="region">
        <textarea type="text" placeholder="都市の概要" v-model="description">
        </textarea>
        <div class="btns">
          <p class="btn" v-on:click="isAdd = false">cancel</p>
          <p class="btn" v-on:click="addCity()">add</p>
        </div>
      </div>
      <div class="bg" v-on:click="isAdd = false"></div>
    </div>
    <!-- added end -->
    <router-view/>
  </div>
</template>

<script>
import { db } from './main'
import firebase from 'firebase'

export default {
  name: 'App',
  data () {
    return {
      isAdd: false,
      name: '',
      nation: '',
      region: '',
      description: '',
      isUser: false,
      user: null
    }
  },
  created () {
    firebase.auth().onAuthStateChanged(user => {
      this.user = user
    })
  },
  methods: {
    addCity () {
      db.collection('cities').add({
        'name': this.name,
        'nation': this.nation,
        'region': this.region,
        'description': this.description,
        'user': 'guest'
      }).then(() => {
        this.isAdd = false
      })
    },
    signInWithGoogle () {
      const provider = new firebase.auth.GoogleAuthProvider()
      firebase.auth().signInWithRedirect(provider).then((result) => {
        this.user = result.user
      })
    },
    signOut () {
      firebase.auth().signOut().then(() => {
        this.user = null
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
  #app
    font-family Avenir, 'Yu Gothic', 'YuGothic', sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin 0
    padding 0

    .btn
      display inline-block
      padding 10px 15px
      background #000
      font-weight bold
      color #fff
      border 1px solid #555
      border-radius 5px
      transition .2s
      cursor pointer

      &:hover
        background #fff
        color #555

    header
      position relative
      margin -8px -8px 0 -8px
      padding-left 20px
      font-size 10px
      border-bottom 1px solid #eee
      text-align left

      h1
        display inline-block

      .btns
        margin-left 20px
        display inline-block

      .sign-in
        background #eee
        position absolute
        top 10px
        right 20px
        height 40px
        width 40px
        border-radius 50%
        line-height 40px
        text-align center
        font-size 20px
        color #fff
        cursor pointer

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
            cursor pointer

        input, textarea
          display block
          width 90%
          margin 10px auto
          font-size 15px
          padding 10px
          border 1px solid #eee
          border-radius 5px

        textarea
          height 100px
          resize none

      .bg
        background rgba(0,0,0,.9)
        position fixed
        top 0
        left 0
        right 0
        bottom 0
        z-index 9
</style>
