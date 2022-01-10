<template>
  <v-card
    class="mx-auto"
    max-width="344"
  >
    <v-card-title>
      {{ info.title }}
    </v-card-title>
  </v-card>
</template>

<script lang="ts">
import Vue, { PropType } from 'vue';
import { Anime } from '@/types/task';

export default Vue.extend({
  props: {
    info: {
      type: Object as PropType<Anime>,
      required: true
    }
  },
})
</script>
