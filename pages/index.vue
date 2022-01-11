<template>
<div>
  <h1>{{ today.getFullYear() }}年{{ cours[today.getMonth()/3] }}放送のアニメ一覧</h1>
  <v-container>
    <v-row>
      <v-col v-for="item in posts" v-bind:key="item.id" cols="12" sm="6" md="6" lg="4" xl="4">
        <anime-coin-panel :info="item" v-on:changeCount="handlChange"/>
      </v-col>
    </v-row>
  </v-container>
  <button @click="twitterShare()">
    Twitter
  </button>
</div>
</template>

<script lang="ts">
import Vue from 'vue';
import { Anime } from '@/types/task';
import AnimeCoinPanel from '~/components/AnimeCoinPanel.vue';

export default Vue.extend({
  components: {
    AnimeCoinPanel: () => import('~/components/AnimeCoinPanel.vue')
  },
  data(){
    return{
      posts: [] as Anime[],
      cours: ["冬", "春", "夏", "秋"],
      coins: new Map<number, number>(),
      today: new Date(),
      title: '今期のアニメコイン',
    }
  },
  methods: {
    handlChange(count: number[]) {
      this.coins.set(count[0], count[1]);
    },
    twitterShare() {
      const baseUrl = 'https://twitter.com/intent/tweet?'
      const text = ['text', this.title + '\n']
      const url = ['url', window.location.href + 'share?']

      let ok = false;
      this.coins.forEach((v, k) => {
        if(v === 0) return;
        ok = true;
        url[1] += `${k}=${v}&`
      })

      if(!ok) {
        window.alert("アニメコインを賭けてください");
        return;
      }

      url[1] = url[1].slice(0, -1);
      const parameter = new URLSearchParams([text, url]).toString()
      const shareUrl = `${baseUrl}${parameter}`
      window.open(shareUrl, 'twitter', 'top=200,left=300,width=600,height=400')
    },
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
