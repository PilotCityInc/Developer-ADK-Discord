<template>
  <v-app>
    <Module :user-doc="userDoc" />
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import Module from './Module/Module.vue';

export default Vue.extend({
  name: 'App',

  components: {
    Module
  },
  setup() {
    const userDoc = {
      data: {
        discordAccessToken: '' // * test by putting your account's access token here
      },
      changeStream: {
        on(event, callback) {
          return {
            fullDocument: {
              discordAccessToken: ''
            }
          };
        }
      }
    };

    return { userDoc };
  }
});
</script>
