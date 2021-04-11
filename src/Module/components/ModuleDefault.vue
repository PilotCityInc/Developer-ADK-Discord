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
      class="mt-3"
      color="#dedede"
      height="2"
      value="100"
      buffer-value="100"
      stream
    />
    <!-- ENTER CONTENT HERE -->
    <!-- DESIGN YOUR ACTIVITY HERE / COMMENT OUT WHEN YOU'VE STARTED DESIGNING -->
    <!-- <div class="module-default__none">Design your activity here</div> -->

    <!-- <v-btn class="mt-2 font-weight-bold" x-large depressed dark color="#ae90b0"
          >Click here when done (Mongo Changestream)</v-btn
        > -->

    <!-- PRE-LINKED DISCORD START -->
    <v-container v-if="!accessToken.length" class="mt-12 d-flex flex-column justify-center pa-6">
      <v-row class="pl-6 pr-6 mb-3 overline d-flex justify-center font-weight-black">
        <!-- Chat with team & PilotCity student community -->
        Join the PilotCity online student community
      </v-row>
      <v-row class="pl-6 pr-6">
        <v-btn
          :disabled="userType === 'stakeholder'"
          rounded
          class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold white--text"
          href="https://discord.com/register?redirect_to=%2Foauth2%2Fauthorize%3Fclient_id%3D818178557939023912%26redirect_uri%3Dhttps%253A%252F%252Fwww.pilotcity.com%252Fdiscordconfirmation%26response_type%3Dcode%26scope%3Didentify%2520guilds.join%2520gdm.join%2520email"
          target="_blank"
          x-large
          depressed
          color="#ae90b0"
          ><v-icon left>mdi-discord</v-icon>Create Discord Account</v-btn
        >
      </v-row>
      <v-row class="pl-6 pr-6">
        <v-btn
          :disabled="userType === 'stakeholder'"
          rounded
          href="https://discord.com/api/oauth2/authorize?client_id=818178557939023912&redirect_uri=https%3A%2F%2Fwww.pilotcity.com%2Fdiscordconfirmation&response_type=code&scope=identify%20guilds.join%20gdm.join%20email"
          target="_blank"
          class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold"
          x-large
          depressed
          outlined
          color="#ae90b0"
          ><v-icon left>mdi-account-circle-outline</v-icon>Connect existing account</v-btn
        >
      </v-row>
    </v-container>
    <!-- PRE-LINKED DISCORD END -->

    <!-- POST-LINKED DISCORD START -->
    <v-container v-if="accessToken.length" class="mt-12 d-flex flex-column justify-center pa-6">
      <v-row v-if="userType !== 'stakeholder'" class="pl-6 pr-6">
        <v-avatar class="ma-3 lighten-2" color="grey" size="60"
          ><img v-if="avatarSource.length" :src="avatarSource"
        /></v-avatar>

        <v-text-field
          class="ma-3"
          append-outer-icon="mdi-discord"
          append
          rounded
          x-large
          readonly
          outlined
          :value="discordUsername"
          label="Your Discord Username"
        ></v-text-field>
      </v-row>
      <v-row v-if="userType === 'stakeholder'" class="pl-6 pr-6">
        <v-avatar class="ma-3 lighten-2" color="grey" size="60"></v-avatar>

        <v-text-field
          class="ma-3"
          append-outer-icon="mdi-discord"
          rounded
          x-large
          readonly
          outlined
          value="Discord username hidden"
          label="Your Discord Username"
        ></v-text-field>
      </v-row>

      <v-row class="pl-6 pr-6">
        <v-btn
          rounded
          href="https://discord.com/channels/688863898686062675/738994257200939068"
          target="_blank"
          :disabled="userType === 'stakeholder'"
          class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold white--text"
          x-large
          depressed
          color="#ae90b0"
          ><v-icon left>mdi-discord</v-icon>Explore Community</v-btn
        >
      </v-row>

      <div class="overline d-flex justify-center font-weight-black mt-6 mb-6">
        <!-- Chat with team & PilotCity student community -->
        Coming Soon
      </div>

      <v-row class="pl-6 pr-6">
        <v-btn
          rounded
          href=""
          target="_blank"
          class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold white--text"
          x-large
          depressed
          :disabled="userType === 'participant' || userType === 'stakeholder'"
          color="#ae90b0"
          ><v-icon left>mdi-account-supervisor-circle</v-icon>Talk to Team</v-btn
        >
      </v-row>
      <v-row class="pl-6 pr-6">
        <v-btn
          rounded
          href=""
          target="_blank"
          :disabled="userType === 'participant' || userType === 'stakeholder'"
          class="module-default__goto-discord ma-2 pl-auto pr-auto font-weight-bold"
          x-large
          depressed
          outlined
          color="#ae90b0"
          ><v-icon left>mdi-account-group</v-icon>Program Participants Chat</v-btn
        >
      </v-row>
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

      <!-- </div> -->

      <!-- <v-btn rounded class="ma-2 font-weight-bold" color="white" x-small depressed
          >Link different Discord account</v-btn
        > -->
    </v-container>

    <!-- POST-LINKED DISCORD END -->

    <!-- Discord to coordinate teams, and post events, communication with employers, link your Discord account below: -->
  </v-container>
</template>

<script lang="ts">
import { ref, reactive, toRefs, PropType, defineComponent } from '@vue/composition-api';
import axios from 'axios';
import { getModAdk } from 'pcv4lib/src';
import Instruct from './ModuleInstruct.vue';
import { MongoDoc } from '../types';

const API_ENDPOINT = 'https://discord.com/api/users/@me';
const CHANNEL_URL = 'https://discord.com/channels/@me/';

export default defineComponent({
  name: 'ModuleDefault',
  components: {
    Instruct
  },
  props: {
    value: {
      required: true,
      type: Object as PropType<MongoDoc>
    },
    userDoc: {
      required: true,
      type: Object as PropType<MongoDoc>
    },
    studentDoc: {
      required: false,
      type: Object as PropType<MongoDoc>,
      default: () => {}
    },
    teamDoc: {
      required: false,
      type: Object as PropType<MongoDoc>,
      default: () => {}
    },
    userType: {
      required: true,
      type: String
      // participant: '',
      // organizer: '',
      // stakeholder: ''
    },
    mongoUser: {
      required: false,
      type: Object,
      default: () => {}
    }
  },
  setup(props, ctx) {
    const state = reactive({
      avatarSource: '',
      discordUsername: 'Username #2938',
      accessToken: '',
      studentAdkIndex: -1
    });

    if (props.studentDoc) {
      const { adkData: studentAdkData, adkIndex: studentAdkIndex } = getModAdk(
        props,
        ctx.emit,
        'community',
        {},
        'studentDoc',
        'inputStudentDoc'
      );
      state.studentAdkIndex = studentAdkIndex;
    }

    const getUser = async () => {
      try {
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
        props.studentDoc!.update(() => ({
          isComplete: true,
          adkIndex: state.studentAdkIndex
        }));
      } catch (err) {
        props.mongoUser?.functions.callFunction(
          'refreshDiscordAccessToken',
          props.userDoc.data._id.toString(),
          props.userDoc.data.discordRefreshToken
        );
      }
    };

    if (props.userDoc.data.discordAccessToken) {
      // when a user already has an access token
      state.accessToken = props.userDoc.data.discordAccessToken;
      getUser();
    }

    props.userDoc.changeStream.next().then((next: Record<string, any>) => {
      const token = next.value.fullDocument.discordAccessToken;
      if (token) {
        state.accessToken = token;
        getUser();
      }
    });

    const getChannelUrl = (id: string) => CHANNEL_URL + id;

    // ! set hrefs like this getChannelUrl(value.data.discordChannelId) for prorgam discord channel
    // ! set hrefs like this getChannelUrl(teamDoc.data.discordChannelId) for team discord channel

    const setupInstructions = ref({
      description: '',
      instructions: ['', '', '']
    });
    const showInstructions = ref(true);
    return {
      ...toRefs(state),
      setupInstructions,
      getChannelUrl,
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

  &__container-discord {
    // width: 100%;
    // padding: none;
    // margin: none;
    margin-top: 0px;
    padding: 0px;
    display: flex;
    max-width: 90%;
    margin-left: auto;
    margin-right: auto;
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
