<template lang="pug">
#app
  h1 Tinkoff ML Case
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
body
  background-color #fefeff
  overflow hidden
  height 100vh
#app
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  h1
    text-align center
    font-size 48px
    font-weight 600
    margin-bottom 36px
  color #2c3e50
  margin-top 60px
  .neo
    border-radius 8px
    box-shadow  4px 4px 9px #d9d9d9, -4px -4px 9px #ffffff
    transition: all ease-out 400ms
    &.active
    &:hover {
      box-shadow: 5px 5px 10px #919191
      -5px -5px 10px #ffffff
}
</style>
