<template>
<div>
  <h1 v-if="!isResult">{{ year }}年{{ cours[cour] }}放送のアニメ一覧</h1>
  <v-btn
    rounded
    outlined
    color="primary"
    @click="viewResult()"
    class="mb-3"
    v-if="!isResult"
  >
    結果を表示する
  </v-btn>
  <v-btn
    rounded
    outlined
    color="danger"
    @click="viewBet()"
    class="mb-3"
    v-else
  >
    コインを賭け直す
  </v-btn>
  <v-container>
    <v-row v-if="!isResult">
      <v-col v-for="item in posts" v-bind:key="item.id" cols="12" sm="6" md="6" lg="4" xl="4">
        <anime-coin-panel :info="item" v-on:changeCount="handlChange"/>
      </v-col>
    </v-row>
    <v-row v-else>
      <v-col cols="12"><h2>あなたは合計{{ sumCoins }}枚のコインを賭けました。</h2></v-col>
      <v-col v-for="item in res" v-bind:key="item.id" cols="12" lg="4" xl="4">
        <anime-panel :title="item.title" :numberOfCoin="coins.get(item.id)"  />
      </v-col>
    </v-row>
  </v-container>
  <v-btn
    rounded
    outlined
    color="primary"
    @click="viewResult()"
    class="mb-3"
    v-if="!isResult"
  >
    結果を表示する
  </v-btn>
  <v-btn
    rounded
    outlined
    color="danger"
    @click="viewBet()"
    class="mb-3"
    v-else
  >
    コインを賭け直す
  </v-btn>
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
      sumCoins: 0,
      today: new Date(),
      isResult: false,
    }
  },
  computed: {
    year(): number {
      return this.today.getFullYear();
    },
    cour(): number {
      return Math.floor(this.today.getMonth()/3);
    }
  },
  methods: {
    handlChange(count: number[]) {
      this.coins.set(count[0], count[1]);
    },
    viewResult(): void { //結果画面の表示
      this.res = [];
      this.posts.map((v) => {
        if(this.coins.get(v.id) !== 0) {
          this.res.push(v);
          this.sumCoins += this.coins.get(v.id) as number;
        }
      })
      if(this.res.length === 0) {
        window.alert("コインを賭けて下さい");
        return;
      }
      else if(this.res.length > 10) {
        window.alert("賭けてるアニメが多すぎます");
        return;
      }
      this.isResult = true;
    },
    viewBet(): void { //賭ける画面の表示
      this.posts.map((x: Anime) => {
        this.coins.set(x.id, 0);
      })
      this.sumCoins = 0;
      this.isResult = false;
    }
  },
  created(): void { //コインのカウントを初期化
    this.$data.posts.map((x: Anime) => {
      this.$data.coins.set(x.id, 0);
    })
  },
  mounted(): void {
    if(this.$data.posts.length === 0)
    {
      window.alert("更新するまで待ってね！");
    }
  },
  async asyncData({ app }){ //APIから今期のアニメデータを取得
    const d = new Date();
    const year = d.getFullYear();
    const cour = Math.floor(d.getMonth()/3) + 1;
    var now = Date.now();
    const url = `https://api.moemoe.tokyo/anime/v1/master/${year}/${cour}?cachebust=${now}`;
    const posts: Anime[] = await app.$axios.$get(url);
    return { posts }
  },
})
</script>
