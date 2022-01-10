<template>
  <v-card
    class="mx-auto"
    max-width="344"
  >
    <v-card-title>
      {{ info.title }}
    </v-card-title>

    <v-btn
      class="mx-2"
      fab
      color="indigo"
      @click="decrementCount()"
    >
      <v-icon dark>
        mdi-minus
      </v-icon>
    </v-btn>
    {{ count }}
    <v-btn
      class="mx-2"
      fab
      color="indigo"
      absolute
      right
      @click="incrementCount()"
    >
      <v-icon dark>
        mdi-plus
      </v-icon>
    </v-btn>
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
  data() {
    return {
      count: 0
    }
  },
  methods: {
    incrementCount(): void {
      ++this.count;
      this.changeCount();
    },
    decrementCount(): void {
      if(this.count == 0) return;
      --this.count;
      this.changeCount();
    },
    changeCount(): void {
      this.$emit('changeCount', this.count);
    }
  }
})
</script>
