<template lang="pug">
.card-client.neo
  .info
    .title Клиент {{ data.id }}
    .subtitle {{ data.gender }} {{ data.marital_status }} {{ data.age }}
  .check
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
  cursor pointer
  .info
    text-align left
    .title
      font-weight 600
    .subtitle
      color #0a0a0a
</style>