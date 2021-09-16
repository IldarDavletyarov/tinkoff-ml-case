<template lang="pug">
.main
  popup-top-categories(:categories="top" :card="activeCard" :is-show="isTopShow" @close="isTopShow = false")
  .wrapper
    .search-side
      .header-wrapper Предсказываем релевантные категории для клиентов Тинькофф-Банка.
        br
        | Попробуйте и проверьте сами
      .input-wrapper
        input(v-model="searchText" type="search" :placeholder="placeholder" :class="{'not-empty': !!searchText.length }")
      .card-wrapper
        .label(:class="{'show': !!searchText.length }") {{ searchLabel }}
        card-client(
          v-for="card in searchResults"
          :key="card.id"
          :data="card"
          :active="activeCard && card.id === activeCard.id"
          :selected="isSelected(card)"
          @click.native="active(card)"
          @select="select"
        )

    .result-side
      transition(name="fade" mode="out-in")
        .selected-menu(v-if="selectedCards.length" key="selected")
          .title {{ selectedTitle }}
          .subtitle id: {{ selectedCards.map(_ => _.id).join(', ') }}
          .neo.button(@click="getMetrics") Получить средние бизнес-метрики
          .neo.button.clear(@click="clear") Снять выделение
        .active-card(v-else-if="activeCard" key="card")
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
          .neo.button.get-top(@click="getTop") Получить список топ-категорий
        .empty(v-else key="empty")
          .label Выберите клиента для более подробной информации о топ-категориях.
          br
          | Выберите сразу несколько для расчета средних бизнес-метрик


</template>
<script>

import mock from '/MOCK_DATA.json';

import CardClient from './card-client.vue';
import PopupTopCategories from './popup-top-categories.vue';

import { pluralize } from '../utils';

export default {
  components: {
    CardClient,
    PopupTopCategories,
  },
  data: () => ({
    results: mock,
    selectedCards: [],
    activeCard: undefined,
    searchText: '',
    isTopShow: false,
    top: [],
  }),
  computed: {
    selectedTitle() {
      const n = this.selectedCards.length
      return `${ pluralize(n,['Выбран', 'Выбрано', 'Выбрано'])} ${ n } ${ pluralize(n, ['клиент', 'клиента', 'клиентов'])} `;
    },
    searchResults() {
      try {
        const regExp = new RegExp(this.searchText);
        return this.results.filter(_ => regExp.test(Object.values(_).join()));
      } catch(e) {
        console.error(e);
        return this.results;
      }
    },
    searchLabel() {
      const n = this.searchResults.length;
      if (!n) {
        return 'Ничего не найдено. Попробуйте поменять запрос';
      } else {
        return `${pluralize(n, ['Найден', 'Найдено', 'Найдено'])} ${ n } ${pluralize(n, ['подходящий клиент', 'подходящих клиента', 'подходящих клиентов'])}`
      }
    },
    placeholder() {
      return `Поиск по регулярке среди ${ this.results.length } ${ pluralize(this.results.length, ['клиента', 'клиентов', 'клиентов']) }`
    }
  },
  methods: {
    active(card) {
      if (this.selectedCards.length) {
        return;
      }
      if (card.id === this.activeCard?.id) {
        this.activeCard = undefined;
      } else {
        this.activeCard = card;
      }
    },
    select(card) {
      if (this.isSelected(card)) {
        this.selectedCards = this.selectedCards.filter(_ => _.id !== card.id);
      } else {
        this.selectedCards.push(card);
      }
    },
    isSelected(card) {
      return !!this.selectedCards.find(_ => _.id === card.id);
    },
    clear() {
      this.selectedCards = [];
    },
    getTop() {
      const top = ['Аптека', 'Банки', 'Здоровье', 'Красота']; // mock
      this.top = top;
      this.isTopShow = true;
    },
    getMetrics() {
      console.log('hello');
    }
  }
}
</script>
<style lang="stylus">

.fade-enter-active
.fade-leave-active
  transition opacity .2s

.fade-enter
.fade-leave-to
  opacity 0


.main
  .wrapper
    max-width 1280px
    width 95%
    margin 0 auto
    display flex
    .search-side
      width 40%
      overflow auto
      height calc(100vh - 350px)
      min-height 100%
      &::-webkit-scrollbar-thumb
        background green
        box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);

      .header-wrapper
        font-weight 500
        font-size 12px
        color #b0b0b0
        margin-bottom 14px
      .input-wrapper
        position sticky
        top 0px
        z-index 1
        padding-right 24px
        width calc(100% - 24px)
        input
          transition all .25s ease-in-out
          display flex
          width calc(100% - 80px)
          height 28px
          margin-bottom 20px
          font-size 18px
          border 1px solid #cecece
          border-radius 8px
          padding 8px 40px
          text-align left
          background hsla(0, 0%, 100%, .5) border-box
          backdrop-filter: blur(4px)
          &.not-empty
          &:focus
            padding 8px
            width calc(100% - 16px)
      .card-wrapper
        padding-right 24px
        position relative
        .card-client
          margin-bottom 12px
        .label
          text-align center
          color #afafaf
          position absolute
          left 50%
          transform translateX(-50%)
          top -16px
          font-size 12px
          opacity 0
          white-space nowrap
          &.show
            opacity 1
            transition all .3s ease-in-out
    .result-side
      border-left 1px solid #f0f0f0
      width 60%
      .empty
        padding 32px
        font-weight 500
        font-size 16px
        color #b0b0b0
        text-align center
        margin-top 20vh
      .selected-menu
        font-size 32px !important
        padding 32px
        .title
          font-size 36px
          letter-spacing 2px
          font-weight 600
        .subtitle
          margin-top 16px
          color #aeaeae
      .active-card
        font-size 32px
        padding 32px
        .title
          font-size 36px
          font-weight 600
          letter-spacing 2px
        .row
          display flex
          margin-top 4px
          .label::after
            content ':'
          .value
            font-weight 600
            margin-left 6px
    @media only screen and (max-width: 960px)
      flex-direction column
      flex-flow column-reverse
      .search-side
        width 100%
        height auto
        .header-wrapper
          display none
        .input-wrapper
          padding 0 8px
          width calc(100% - 16px)
          input
            padding 8px
            width calc(100% - 16px)
        .card-wrapper
          padding 0 8px
      .result-side
        width 100%
        border-left none
        .empty
        .active-card
        .selected-menu
          font-size 18px
          min-height 204px
          padding 8px
          margin 0
          .title
            font-size 24px
        .empty
          text-align left
          font-size 24px
          font-weight 600
          font-family Helvetica
          letter-spacing 1px
  
  .button
    cursor pointer
    margin-top 32px
    min-height 60px
    text-align center
    vertical-align center
    line-height 60px
    background #f9de56
    border-radius 16px !important
    font-size 32px
    white-space nowrap
    @media only screen and (max-width: 960px)
      font-size 18px
      min-height 48px
      line-height 48px
      border-radius 12px !important
      margin-top 16px
      
    &.clear
      background #fff
      border 1px solid #aeaeae
      color #aeaeae
      &:hover
        border 1px solid #000
        color #000


</style>