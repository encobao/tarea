<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
      height="10"
      extension-height="80"
      absolute
    >
      <!-- <v-spacer></v-spacer>
      
      <v-toolbar-title class="text-h3">
        TAREA
      </v-toolbar-title>

      <v-spacer></v-spacer> -->

      <template v-slot:extension>
        <v-tabs
          v-model="tabs"
          fixed-tabs
          icons-and-text
        >
          <v-tabs-slider color="secondary"></v-tabs-slider>

          <v-tab
            v-for="tabName in tabNames"
            :key="tabName.id"
          >
            {{ tabName.name }}
            <v-icon>{{ tabName.icon }}</v-icon>
          </v-tab>
        </v-tabs>
      </template>

    </v-app-bar>

    <v-main>
      <v-sheet
        color="white"
        max-width="600"
        class="mx-auto my-5"
      >
        <v-tabs-items v-model="tabs">

          <v-tab-item class="px-8 pt-6">
            <!-- <v-container>
              <v-row
                v-for="question in questionList"
                :key="question.id"
                class="text-h4"
              >
                <v-col 
                  cols="6"
                  class="text-right px-0 pt-2"
                >
                  {{ question.formula }} = 
                </v-col>

                <v-col
                  cols="6"
                  class="pr-0 py-0"
                > -->
                  <math-question
                    v-for="question in questionList"
                    :key="question.id"
                    :question="question"
                    @enviar-click="onEnviarClick"
                  >
                  </math-question>
                <!-- </v-col>

              </v-row>
            </v-container> -->
          </v-tab-item>

          <v-tab-item>
            <reading-expansion-panel :readingList="readingList">
            </reading-expansion-panel>
          </v-tab-item>
        </v-tabs-items>

        <v-dialog
          v-model="dialog"
          width="350"
        >
          <v-card>
            <v-card-title class="text-h5">
              Inténtalo de nuevo
            </v-card-title>

            <v-divider></v-divider>
    
            <v-img
              :src="ramdomImage()"
              width="250"
              class="mx-auto"
            ></v-img>

            <v-divider></v-divider>
    
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="primary"
                text
                @click="dialog = false"
                class="text-h6"
              >
                ok
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

      </v-sheet>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import ReadingExpansionPanel from './components/ReadingExpansionPanel.vue';
import MathQuestion from './components/MathQuestion.vue';

export default {
  name: 'App',

  components: {
    MathQuestion,
    ReadingExpansionPanel,
  },

  data: () => ({
    tabs: null,
    dialog: false,
    apiErrored: false,
    //imgReadingPath: process.env.VUE_APP_IMG_READING_PATH,
    tabNames: [
      {
        id: 0,
        name: 'Matematica',
        icon: 'mdi-counter'
      },
      {
        id: 1,
        name: 'Lectura',
        icon: 'mdi-format-letter-case'
      },
    ],
    questionList: [
      {
        id: 0,
        formula: '2+5',
        result: 7,
        completed: false,
      },
    ],
    emojiList: [
      { name: "wa_emoji1.jpg" },
    ],
    readingList: [
      {
        id: 0,
        title: 'Los desayunos de Laura',
        paras: [
            { id: 0, text : "A las ocho de la mañana la mamá de Laura ya se ha tomado su café con tostadas." },
        ],
      },
    ],
  }),
  methods: {
    onEnviarClick(result, id) {
      let index = this.questionList.findIndex( question => question.id == id )

      if ( this.questionList[index].result == result ) {
        this.questionList[index].completed = true
      } else {
        this.dialog = true
      }
    },

    async getApiObject(apiLocation) {
      try {
        const resp = await axios.get(apiLocation)
        return resp.data
      } catch(error) {
          this.apiErrored = true
      }
    },    

    ramdomImage() {
      let index = Math.floor(Math.random() * this.emojiList.length)
      return process.env.VUE_APP_IMG_EMOJI_PATH + this.emojiList[index].name
    }
  },

  mounted () {
    this.getApiObject(process.env.VUE_APP_API_PATH + 'emoList.json')
      .then(data => this.emojiList = data)

    this.getApiObject(process.env.VUE_APP_API_PATH + 'questionList.json')
      .then(data => this.questionList = data)

    this.getApiObject(process.env.VUE_APP_API_PATH + 'readingList.json')
      .then(data => this.readingList = data)
  },  
};
</script>

