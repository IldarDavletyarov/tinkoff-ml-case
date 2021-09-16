<template lang="pug">
#app
  header
    picture
      source(srcset="logo-small.png" media="(max-width: 960px)")
      source(srcset="logo.png")
      img(srcset="logo.png")
    h1 ML Case
  main-form
</template>

<script>
import MainForm from './components/main.vue'

export default {
  name: 'App',
  components: {
    MainForm
  }
}
</script>

<style lang="stylus">
@import url(./reset.styl);
html
html body
  background-color #fefeff
  margin 0
  padding 0
  @media only screen and (min-width: 961px)
    overflow hidden
    height 100vh
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  header
    img
      max-width 280px
      margin 40px auto 0
      display block
    h1
      text-align center
      font-size 48px
      font-weight 600
      margin-bottom 36px
      white-space nowrap
      margin-top 10px
      color #000
    @media only screen and (max-width: 960px)
      img
        max-width 50vw
        margin 10px auto

      h1
        line-height 34px
        margin 10px
        color #000
  color #2c3e50
  .neo
    border-radius 8px
    box-shadow  4px 4px 9px #d9d9d9, -4px -4px 9px #ffffff
    transition: all ease-out 400ms
    &.active
    &:hover
      box-shadow 5px 5px 10px #919191
      -5px -5px 10px #ffffff

.no-select
  -webkit-touch-callout none
  -webkit-user-select none
  -khtml-user-select none
  -moz-user-select none 
  -ms-user-select none
  user-select none
  ::selection
    background-color none
</style>
