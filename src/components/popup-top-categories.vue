<template lang="pug">
.popup-top-categories(v-if="isShow" @click.self="$emit('close')")
  .content
    .title Топ-категории
    .categories
      .category(v-for="category in results" :key="category") {{ category }}
    .actions
      .button.neo.get-full(@click="getFull" v-if="!additional.length") Получить полный список
</template>
<script>
export default {
  props: {
    categories: {
      type: Array,
      default: [],
    },
    card: {
      type: Object,
      default: () => ({}),
    },
    isShow: {
      type: Boolean,
      default: false,
    },
  },
  data: () => ({
    additional: [],
  }),
  watch: {
    isShow() {
      this.additional = []; // reset
    },
  },
  computed: {
    results() {
      return [...this.categories, ...this.additional];
    },
  },
  methods: {
    getFull() {
      this.additional = Array(10).fill('hello');
    }
  }
};
</script>
<style lang="stylus">
.popup-top-categories
  background hsla(0, 0%, 100%, .5) border-box
  -webkit-backdrop-filter: blur(2px);
  position fixed
  height 100vh
  top 0
  left 0
  width 100vw
  z-index 100
  .content
    transition all .3s ease-in
    position absolute
    width 95%
    max-width 480px
    max-height 80vh
    overflow auto
    background #fff
    border-radius 16px
    top 50%
    left 50%
    transform translate(-50%, -50%)
    padding 24px
    box-shadow 0px 0px 50px 0px rgba(0, 0, 0, 0.2)
    .title
      font-size 32px
      font-weight 600
      text-align center
    .categories
      text-align center
      .category
        font-size 28px
        margin-top 20px
        position relative
        &:not(:last-child)::after
          content ''
          position absolute
          background #a0a0a0
          height 0.5px
          width 160px
          bottom -13px
          left 50%
          transform translateX(-50%)
    .actions
      .button.neo.get-full
        min-height 48px
        line-height 48px
        border-radius 10px !important
        font-weight 500
        font-size 20px

</style>