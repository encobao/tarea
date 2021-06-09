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
          
          <v-tab-item>
            <mult-expansion-panel :multChartImgs='multTableImgs'></mult-expansion-panel>
          </v-tab-item>

          <v-tab-item>
            <math-expansion-panel 
              :operationList="mathOperationList"
              @enviar-click="onEnviarClick"
            >
            </math-expansion-panel>
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
import MathExpansionPanel from './components/MathExpansionPanel.vue';
import MultExpansionPanel from './components/MultExpansionPanel.vue';

export default {
  name: 'App',

  components: {
    ReadingExpansionPanel,
    MathExpansionPanel,
    MultExpansionPanel,
  },

  data: () => ({
    tabs: null,
    dialog: false,
    apiErrored: false,
    
    multTableImgs: [
      { 
        name: "Tabla del 1",
        img: "chart1.jpg"
      },
      { 
        name: "Tabla del 2",
        img: "chart2.jpg"
      },
    ],
    
    tabNames: [
      {
        id: 0,
        name: 'Tablas',
        icon: 'mdi-close-thick'
      },
      {
        id: 1,
        name: 'Matematica',
        icon: 'mdi-counter'
      },
      {
        id: 2,
        name: 'Lectura',
        icon: 'mdi-format-letter-case'
      },
    ],
    
    mathOperationList: [
      {
        id: 0,
        type: "Suma",
        list:[
          {
              id: 0,
              formula: "8+4",
              result: 12,
              completed: false
          },
          {
              id: 1,
              formula: "9+1",
              result: 10,
              completed: false
          }
        ]
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

    processAnswer (result, formulaId, operationId) {
      let opIndex = this.mathOperationList.findIndex( op => op.id == operationId )

      let formulaIndex = this.mathOperationList[opIndex].list.findIndex( question => question.id == formulaId )

      if ( this.mathOperationList[opIndex].list[formulaIndex].result == result ) {
        this.mathOperationList[opIndex].list[formulaIndex].completed = true
        this.succAnswer()
      } else {
        this.wrongAnswer ()
      }
    },

    wrongAnswer () {
      this.audioWrongAnswer.play()
      this.dialog = true
    },

    succAnswer () {
      const { totalCompleted, total } = this.countCompleted()
      totalCompleted == total ? this.audioMathCompleted.play() : this.audioSuccAnswer.play()
    },

    countCompleted () {
        let total = 0
        let totalCompleted  = 0

        this.mathOperationList.forEach(element => {
          total += element.list.length
          totalCompleted += element.list.reduce((subtotal, v) => (v.completed === true ? subtotal + 1 : subtotal), 0)

        });
        return { totalCompleted: totalCompleted, total: total }
    },

    onEnviarClick(result, formulaId, operationId) {
      this.processAnswer(result, formulaId, operationId)
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
      return process.env.VUE_APP_IMG_EMOJI_FOLDER + this.emojiList[index].name
    },
  
  },

  mounted () {

    this.getApiObject(process.env.VUE_APP_EMOJI_FILE)
      .then(data => this.emojiList = data)

    this.getApiObject(process.env.VUE_APP_QUESTION_LIST_FILE)
      .then(data => this.mathOperationList = data)

    this.getApiObject(process.env.VUE_APP_READING_LIST_FILE)
      .then(data => this.readingList = data)

    this.getApiObject(process.env.VUE_APP_MULT_TABLE_FILE)
      .then(data => this.multTableImgs = data)
  }, 

  created () {
    this.audioSuccAnswer = new Audio(process.env.VUE_APP_SUCC_ANSWER_SOUND_FILE);
    this.audioMathCompleted = new Audio(process.env.VUE_APP_MATH_COMPLETED_SOUND_FILE);
    this.audioWrongAnswer = new Audio(process.env.VUE_APP_WRONG_ANSWER_SOUND_FILE);
  },

};
</script>

