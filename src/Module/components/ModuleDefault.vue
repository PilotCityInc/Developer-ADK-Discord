<template>
  <v-container class="module-default pa-0">
    <div class="module-default__instructions">
      <v-expansion-panels v-model="showInstructions" class="module-default__instructions" flat>
        <v-expansion-panel>
          <v-expansion-panel-header
            v-show="showInstructions"
            hide-actions
            class="pa-0"
            @click="showInstructions = true"
          >
            <template v-slot="{ open }">
              <v-scroll-y-transition hide-on-leave>
                <div v-if="!open" class="d-flex flex-column justify-center">
                  <v-icon color="grey lighten-2" class="d-flex justify-center">
                    mdi-chevron-down
                  </v-icon>
                  <div color="grey lighten-2" class="module-default__collapse-title">
                    INSTRUCTIONS
                  </div>
                </div>
              </v-scroll-y-transition>
            </template>
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            <Instruct readonly />
            <div @click="showInstructions = true">
              <div class="module-default__collapse-title">CLOSE</div>
              <!-- <div class="hr"/> OPTIONAL -->
              <v-icon color="grey lighten-2" class="d-flex justify-center"> mdi-chevron-up </v-icon>
            </div>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </div>

    <v-progress-linear
      class="module-default__collapse-divider"
      color="#dedede"
      height="2"
      value="100"
      buffer-value="100"
      stream
    />
    <div class="module-edit__container mt-12">
      <!-- ENTER CONTENT HERE -->
      <!-- DESIGN YOUR ACTIVITY HERE / COMMENT OUT WHEN YOU'VE STARTED DESIGNING -->
      <!-- <div class="module-default__none">Design your activity here</div> -->

      <!-- <v-btn class="mt-2 font-weight-bold" x-large depressed dark color="#ae90b0"
          >Click here when done (Mongo Changestream)</v-btn
        > -->

      <!-- PRE-LINKED DISCORD START -->
      <v-container v-if="!accessToken.length">
        <div class="overline d-flex justify-center font-weight-bold">
          <!-- Chat with team & PilotCity student community -->
          Join the PilotCity online student community
        </div>
        <a
          href="https://discord.com/register?redirect_to=%2Foauth2%2Fauthorize%3Fclient_id%3D818178557939023912%26redirect_uri%3Dhttps%253A%252F%252Fpilotcity.com%252Fdiscordconfirmation%26response_type%3Dcode%26scope%3Didentify%2520email%2520guilds.join%2520gdm.join"
          style="text-decoration: none"
          target="_blank"
        >
          <v-btn
            rounded
            class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold"
            x-large
            depressed
            dark
            color="#ae90b0"
            ><v-icon left>mdi-discord</v-icon>Create Discord Account</v-btn
          ></a
        >
        <a
          href="https://discord.com/api/oauth2/authorize?client_id=818178557939023912&redirect_uri=https%3A%2F%2Fpilotcity.com%2Fdiscordconfirmation&response_type=code&scope=identify%20email%20guilds.join%20gdm.join"
          style="text-decoration: none"
          target="_blank"
        >
          <v-btn
            rounded
            class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold"
            x-large
            depressed
            outlined
            color="#ae90b0"
            >Connect existing account</v-btn
          ></a
        >
      </v-container>
      <!-- PRE-LINKED DISCORD END -->

      <!-- POST-LINKED DISCORD START -->
      <v-container v-if="accessToken.length">
        <v-row>
          <v-avatar class="ma-3 lighten-2" color="grey" size="60"
            ><img v-if="avatarSource.length" :src="avatarSource"
          /></v-avatar>

          <v-text-field
            class="ma-3"
            append-outer-icon="mdi-discord"
            rounded
            x-large
            readonly
            outlined
            :value="discordUsername"
            label="Your Discord Username"
          ></v-text-field>
        </v-row>

        <!-- <v-btn
          rounded
          class="ma-2 pl-auto pr-auto font-weight-bold"
          x-large
          depressed
          dark
          color="#ae90b0"
          ><v-icon left>mdi-account-supervisor-circle</v-icon>Go to Team Chat</v-btn
        > -->

        <!-- <v-btn
          rounded
          outlined
          class="ma-2 pl-auto pr-auto font-weight-bold"
          x-large
          depressed
          dark
          color="#ae90b0"
          >Chat with everyone in Program</v-btn
        > -->
        <!-- <div class="d-flex justify-center"> -->
        <a
          href="https://discord.com/channels/688863898686062675/738994257200939068"
          style="text-decoration: none"
          target="_blank"
        >
          <v-btn
            rounded
            class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold"
            x-large
            depressed
            dark
            color="#ae90b0"
            ><v-icon left>mdi-account-supervisor-circle</v-icon>Go to PilotCity Discord</v-btn
          ></a
        >
        <!-- </div> -->

        <!-- <v-btn rounded class="ma-2 font-weight-bold" color="white" x-small depressed
          >Link different Discord account</v-btn
        > -->
      </v-container>

      <!-- POST-LINKED DISCORD END -->

      <!-- Discord to coordinate teams, and post events, communication with employers, link your Discord account below: -->
    </div>
  </v-container>
</template>

<script lang="ts">
import { ref, reactive, toRefs, PropType, defineComponent } from '@vue/composition-api';
import axios from 'axios';
import Instruct from './ModuleInstruct.vue';
import { MongoDoc } from '../types';

const API_ENDPOINT = 'https://discord.com/api/users/@me';

export default defineComponent({
  name: 'ModuleDefault',
  components: {
    Instruct
  },
  props: {
    userDoc: {
      required: true,
      type: Object as PropType<MongoDoc>
    }
  },
  setup(props) {
    const state = reactive({
      avatarSource: '',
      discordUsername: 'Username #2938',
      accessToken: ''
    });

    const getUser = async () => {
      const {
        data: { id, avatar, username, discriminator }
      } = await axios.get(API_ENDPOINT, {
        headers: { Authorization: `Bearer ${state.accessToken}` }
      });
      state.avatarSource =
        avatar !== null
          ? `https://cdn.discordapp.com/avatars/${id}/${avatar}.png`
          : 'https://discordapp.com/assets/322c936a8c8be1b803cd94861bdfa868.png';

      state.discordUsername = `${username} #${discriminator}`;
    };

    if (props.userDoc.data.discordAccessToken) {
      // when a user already has an access token
      state.accessToken = props.userDoc.data.discordAccessToken;
      getUser();
    }

    props.userDoc.changeStream.on('change', ({ fullDocument }) => {
      state.accessToken = fullDocument.accessToken;
      getUser();
    });

    // ! if no changeStream, use this with a db call
    // window.onfocus = () => {
    //   getUser();
    // };

    // window.onblur = () => {
    //   getUser();
    // };

    const setupInstructions = ref({
      description: '',
      instructions: ['', '', '']
    });
    const showInstructions = ref(true);
    return {
      ...toRefs(state),
      setupInstructions,
      showInstructions
    };
  }
});
</script>

<style lang="scss">
.module-default {
  &__goto-discord {
    width: 100%;
  }
  &__none {
    border-radius: 5px;
    // border: 1px solid #dedede;
    height: 100px;
    text-align: center;
    background-color: #dedede;
    font-weight: 700;
    color: #ffffff;
    font-size: 18px;
    padding-top: 35px;
  }

  &__collapse-divider {
    margin-top: 15px;
    // margin-bottom: 75px;
    margin-right: none;
    margin-left: none;
    padding-right: none;
    padding-left: none;
    width: 100%;
  }

  &__collapse-title {
    color: #dedede;
    text-align: center;
    text-transform: uppercase;
    font-weight: 900;
    letter-spacing: 1px;
    font-size: 13px;
    //  text-uppercase font-weight-bold text-subtitle-2 text-center
  }

  &__container {
    // width: 100%;
    // padding: none;
    // margin: none;
    margin-top: 0px;
    padding: 0px;
  }
  &__employer-title {
    font-size: 25px;
    font-weight: 700;
  }

  &__scope {
    font-size: 22px;
    font-weight: 800;
    text-align: center;
    max-width: 95%;
    margin: auto;
  }
  &__youtube {
    height: 400px;
    width: 95%;
    border-radius: 25px;
    // margin: 0px;
    background-color: #dedede;

    // text-align: center;
    // justify-content: center;
    // align-items: center;
    // padding-top: auto;
    // padding-bottom: auto;
  }
  &__about {
    font-size: 15px;
    font-weight: 700;
    text-align: center;
    max-width: 90%;
    margin: auto;
    line-height: 30px;
  }

  &__faq {
    font-size: 15px;
    font-weight: 700;
    text-align: center;
    max-width: 90%;
    margin: auto;
    line-height: 30px;
  }

  &__faq-chat {
    display: flex;
    flex-direction: column;
    text-align: left;
    margin-bottom: 5%;
  }
  &__faq-chat-line {
    margin: 5px;
  }

  &__faq-avatar {
    margin: 5px;
  }

  &__faq-question {
    // text-align: left;
    font-family: 'Raleway';
    font-size: 16px;
    font-weight: 800;
    color: #404142;
  }

  &__faq-answer {
    text-align: left;
    font-family: 'Raleway';
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 0px;
    color: white;
    font-style: italic;
  }

  &__faq-answer-dark {
    text-align: left;
    font-family: 'Raleway';
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 0px;
    color: #404142;
    font-style: italic;
  }

  &__faq-answer-dark-highlight {
    text-align: left;
    font-family: 'Raleway';
    font-size: 13px;
    font-weight: 800;
    letter-spacing: 0px;
    color: #404142;
  }

  &__specs-title {
    font-weight: 800;
  }
}
</style>
