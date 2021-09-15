<template lang="pug">
.main
  .wrapper
    .search-side
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
        .title active {{ activeCard.id }}
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
      .input-wrapper
        width 100%
        input
         display flex
         width 100%
         min-height 40px
         font-size 24px

</style>