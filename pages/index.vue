<template>
<div>
  <h1>{{ new Date().getFullYear() }}年放送のアニメ一覧</h1>
  <ol>
    <li v-for="item in posts" v-bind:key="item.id">{{ item.title }}</li>
  </ol>
</div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  data(){
    return{
      posts:[],
    }
  },
  async asyncData({ app }){
    const url = "https://api.moemoe.tokyo/anime/v1/master/2022/1"
    const posts = await app.$axios.$get(url)
    return { posts }
  },
})
</script>
