<template>
  <v-card
    color="indigo lighten-5"
    min-height="120"
  >
    <v-card-title class="layout justify-center">
      {{ title }}
    </v-card-title>

<v-layout justify-center>
    <v-card-text >
      <v-row>
        <v-col>
          <span
            class="text-h2"
            v-text="numberOfCoin"
          ></span>
          <span class="subheading mr-1">枚</span>
        </v-col>
      </v-row>
    </v-card-text>
</v-layout>
  </v-card>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: {
    title: {
      type: String,
      required: true
    },
    numberOfCoin: {
      type: Number,
      required: true
    },
  },
  data() {
    return {
    }
  },
})
</script>
