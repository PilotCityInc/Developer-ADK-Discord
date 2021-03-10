<template>
  <v-app>
    <Module :db="db" />
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import { Db } from 'mongodb';
import Module from './Module/Module.vue';

export default Vue.extend({
  name: 'App',

  components: {
    Module
  },
  setup() {
    const db: Db = {
      collection(name) {
        return {
          findOne(query) {
            return {
              discordAccessToken: ''
            };
          }
        };
      }
    };
    return { db };
  }
});
</script>
