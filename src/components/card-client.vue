<template lang="pug">
.card-client.neo(:class="{'active-client': active, 'selected': selected }")
  .info
    .title Клиент {{ data.id }}
    .subtitle {{ data.gender }} {{ data.marital_status }} {{ data.age }}
  .check(@click.stop="$emit('select', data)")
    svg(xmlns="http://www.w3.org/2000/svg" width="26" height="26" viewBox="-4 -4 32 32")
      path(d="M9 22l-10-10.598 2.798-2.859 7.149 7.473 13.144-14.016 2.909 2.806z")
</template>
<script>
export default {
  props: {
    data: {
      type: Object,
      default: () => ({}),
    },
    active: {
      type: Boolean,
      default: false,
    },
    selected: {
      type: Boolean,
      default: false,
    },
    number: {
      type: Number,
      default: 0,
    },
  },
};
</script>
<style lang="stylus">
.card-client.neo
  height 42px
  padding 8px 16px
  position relative
  cursor pointer
  border-left 0px solid black
  &.active-client
    box-sizing content-box
    border-left 6px solid black
  .info
    text-align left
    .title
      font-weight 600
    .subtitle
      color #0a0a0a
  .check
    position absolute
    transition all .3s ease-in-out
    right 14px
    top 50%
    transform translateY(-50%)
    height 28px
    width 28px
    border 1px solid #c0c0c0
    border-radius 5px
    svg
      transition all .3s ease-in-out
      opacity 0
      margin-top 1px
      margin-left 1px
    &:hover
      border 1px solid #aeaeae
      svg
        opacity .2
  &.selected
    .check
      border 1px solid #000
      svg
        opacity 1
      
</style>