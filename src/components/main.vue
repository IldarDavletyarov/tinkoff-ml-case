<template lang="pug">
.main
  .wrapper
    .search-side
      .header-wrapper Выберите клиента для более подробной информации о топ-категориях.
        br
        | Выберите сразу несколько для расчета средних бизнес-метрик
      .input-wrapper
        input(v-model="searchText" type="search" :placeholder="placeholder")
      .card-wrapper
        card-client(
          v-for="card in searchResults"
          :key="card.id"
          :data="card"
          :active="activeCard && card.id === activeCard.id"
          :selected="isSelected(card)"
          @click.native="active(card)"
          @select="select(card)"
        )  

    .result-side
      .active-card(v-if="activeCard")
        .title Клиент {{ activeCard.id }}
        .row
          .label Пол
          .value {{ activeCard.gender }}
        .row
          .label Возраст
          .value {{ activeCard.age }}
        .row
          .label Семейное положение
          .value {{ activeCard.marital_status }}
        .row
          .label Количество детей
          .value {{ activeCard.amount_children }}
        .neo.button.get-top Получить список топ-категорий  
</template>
<script>

import mock from '/MOCK_DATA.json';

import CardClient from './card-client.vue';

import { pluralize } from '../utils';

export default {
  components: {
    CardClient,
  },
  data: () => ({
    results: mock,
    selectedCards: [],
    activeCard: undefined,
    searchText: '',
  }),
  computed: {
    searchResults() {
      try {
        const regExp = new RegExp(this.searchText);
        return this.results.filter(_ => regExp.test(JSON.stringify(_)));
      } catch(e) {
        console.error(e);
        return this.results;
      }
    },
    placeholder() {
      return `Поиск по регулярке среди ${ this.results.length } ${ pluralize(this.results.length, ['клиента', 'клиентов', 'клиентов']) }`
    }
  },
  methods: {
    active(card) {
      this.activeCard = card;
    },
    select(card) {
      this.selectedCards.push(card);
    },
    isSelected(card) {
      return this.selectedCards.find(_ => _.id === card.id)?.length;
    }
  }
}
</script>
<style lang="stylus">
.main
  .wrapper
    max-width 1280px
    width 95%
    margin 0 auto
    display flex
    .search-side
      width 40%
      overflow auto
      height 100vh
      .header-wrapper
        font-weight 500
        font-family Avenir
        font-size 14px
        color #b0b0b0
        margin-bottom 14px
      .input-wrapper
        position sticky
        top 0px
        background #fff
        z-index 1
        padding-right 16px
        width calc(100% - 16px)
        box-shadow 0px 28px 11px -3px rgba(255, 255, 255, 0.5)
        input
          display flex
          width calc(100% - 16px)
          height 20px
          margin-bottom 20px
          font-size 20px
          border 0.5px solid grey
          border-radius 8px
          padding 8px
          text-align center
      .card-wrapper
        padding-right 16px
        .card-client
          margin-bottom 8px
    .result-side
      border-left 1px solid #f0f0f0
      width 40%
      .active-card
        font-size 32px
        padding 32px
        .title
          font-size 36px
          font-weight 600
        .row
          display flex
          .label::after
            content ':'
          .value
            font-weight 600
            margin-left 6px
        .button
          cursor pointer
          &.get-top
            margin-top 32px
            height 60px
            text-align center
            vertical-align center
            line-height 60px
            background #f9de56
            border-radius 16px !important


</style>