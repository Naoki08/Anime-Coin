<template>
<div>
  <h1>{{ today.getFullYear() }}年{{ cours[today.getMonth()/3] }}放送のアニメ一覧</h1>
  <v-container>
    <v-row>
      <v-col v-for="item in posts" v-bind:key="item.id" cols="12" sm="6" md="6" lg="4" xl="4">
        <anime-card :info="item" v-on:changeCount="handlChange"/>
      </v-col>
    </v-row>
  </v-container>
</div>
</template>

<script lang="ts">
import Vue from 'vue';
import { Anime } from '@/types/task';

export default Vue.extend({
  components: {
    AnimeCard: () => import('~/components/AnimeCard.vue')
  },
  data(){
    return{
      cours: ["冬", "春", "夏", "秋"],
      today: new Date()
    }
  },
  methods: {
    handlChange(count: number) {
    }
  },
  async asyncData({ app }){
    const d = new Date();
    const url = `https://api.moemoe.tokyo/anime/v1/master/${d.getFullYear()}/${(d.getMonth() / 3) + 1}`;
    const posts: Anime[] = await app.$axios.$get(url);
    return { posts }
  },
})
</script>
