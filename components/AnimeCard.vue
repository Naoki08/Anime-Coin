<template>
  <v-card
    color="indigo lighten-5"
    min-height="300"
  >
    <v-card-title class="layout justify-center">
      {{ info.title }}
    </v-card-title>

    <v-card-text>
      <v-row
        class="mb-4"
        justify="space-between"
      >
        <v-col class="text-left">
          <span
            class="text-h2 font-weight-light"
            v-text="count"
          ></span>
          <span class="subheading font-weight-light mr-1">枚</span>
        </v-col>
      </v-row>

      <v-slider
        v-model="count"
        min="0"
        max="50"
        @change="changeCount()"
      >
        <template v-slot:prepend>
          <v-icon
            @click="decrement"
          >
            mdi-minus
          </v-icon>
        </template>

        <template v-slot:append>
          <v-icon
            @click="increment"
          >
            mdi-plus
          </v-icon>
        </template>
      </v-slider>
    </v-card-text>
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
    increment(): void {
      ++this.count;
      this.changeCount();
    },
    decrement(): void {
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
