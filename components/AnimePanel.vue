<template>
  <v-card
    outlined
    color="teal lighten-4"
  >
    <v-layout>
        <v-card-text>
          <v-row>
            <v-col cols="9">{{ title }}</v-col>
            <v-col cols="3">
              <span
                class="text-h6"
                v-text="numberOfCoin"
              ></span>
              <span>枚</span>
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
