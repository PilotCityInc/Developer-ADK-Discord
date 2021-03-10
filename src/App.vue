<template>
  <v-app>
    <Module :user-doc="userDoc" :user-type="userTypeStub" />
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
        discordAccessToken: 'zWXhmAgKJtSN461vUWv3f1jmBUjK68' // * test by putting your account's access token here
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
    const userTypeStub = 'organizer';

    return { userDoc, userTypeStub };
  }
});
</script>
