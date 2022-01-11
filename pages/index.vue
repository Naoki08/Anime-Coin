<template>
<div>
  <h1 v-if="!isResult">{{ today.getFullYear() }}年{{ cours[today.getMonth()/3] }}放送のアニメ一覧</h1>
  <h1 v-else>今期のアニメコイン</h1>
  <v-container>
    <v-btn
      rounded
      outlined
      color="primary"
      @click="viewResult()"
      class="mb-3"
      v-show="!isResult"
    >
      結果を表示する
    </v-btn>
    <v-row v-if="!isResult">
      <v-col v-for="item in posts" v-bind:key="item.id" cols="12" sm="6" md="6" lg="4" xl="4">
        <anime-coin-panel :info="item" v-on:changeCount="handlChange"/>
      </v-col>
      <v-btn
        rounded
        outlined
        color="primary"
        @click="viewResult()"
      >
        結果を表示する
      </v-btn>
    </v-row>
    <v-row v-else>
      <v-col v-for="item in res" v-bind:key="item.id" cols="6" lg="4" xl="4">
        <anime-panel :title="item.title" :numberOfCoin="coins.get(item.id)"  />
      </v-col>
      <h2>※やり直す場合はリロードしてください</h2>
    </v-row>
  </v-container>
</div>
</template>

<script lang="ts">
import Vue from 'vue';
import { Anime } from '@/types/task';

export default Vue.extend({
  components: {
    AnimeCoinPanel: () => import('~/components/AnimeCoinPanel.vue'),
    AnimePanel: () => import('~/components/AnimePanel.vue')
  },
  data(){
    return{
      posts: [] as Anime[],
      res: [] as Anime[],
      cours: ["冬", "春", "夏", "秋"],
      coins: new Map<number, number>(),
      today: new Date(),
      isResult: false,
    }
  },
  methods: {
    handlChange(count: number[]) {
      this.coins.set(count[0], count[1]);
    },
    viewResult(): void{
      this.res = [];
      this.posts.map((v) => {
        if(this.coins.get(v.id) !== 0) {
          this.res.push(v);
        }
      })
      if(this.res.length === 0) {
        window.alert("アニメコインを賭けて下さい");
        return;
      }
      else if(this.res.length > 10) {
        window.alert("賭けてるアニメが多すぎます");
        return;
      }
      this.isResult = true;
    }
  },
  created(): void {
    this.$data.posts.map((x: Anime) => {
      this.$data.coins.set(x.id, 0);
    })
  },
  async asyncData({ app }){
    const d = new Date();
    const url = `https://api.moemoe.tokyo/anime/v1/master/${d.getFullYear()}/${(d.getMonth() / 3) + 1}`;
    const posts: Anime[] = await app.$axios.$get(url);
    return { posts }
  },
})
</script>
