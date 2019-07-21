<template>
  <div id="app">
    <header>
      <h1>City.</h1>
      <div class="btns">
        <p class="btn">都市を追加する</p>
      </div>
      <div class="sign-in">?</div>
    </header>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
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
</style>
