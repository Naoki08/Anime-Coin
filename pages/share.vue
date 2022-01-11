<template>
<div>
  <h1>Share Page</h1>
  <anime-panel title="hoge" :numberOfCoin=3 url="fuga" />
</div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  components: {
    AnimePanel: () => import('~/components/AnimePanel.vue')
  },
  data(){
    return {

    }
  },
  methods: {},
  mounted() {
    console.log(this.$route.query);
  }
})
</script>
