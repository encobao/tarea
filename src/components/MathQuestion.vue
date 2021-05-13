<template>
  <v-container>
    <v-row
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
      >
        <v-form>
          <div 
            v-if="!question.completed"
            class="d-flex"
          >
            <v-text-field 
              v-model="result"
              class="text-h5 pr-4"
              type="number"
              outlined
            >
            </v-text-field>
            <v-btn
              color="primary"
              class="mt-2 py-5"
              @click="$emit('enviar-click', result, question.id)"
              elevation="5"
              small
            >
              ok
            </v-btn>
          </div>

          <div 
            v-else
            class="d-flex justify-space-between"
          >
            <div
              style="height: 88px;"
              class="text-h4 pr-9 pt-2"
            >
              {{ question.result }}
            </div>
            <v-icon
              color="secondary"
              class="mr-5 mt-n7"
              x-large
            >
              mdi-emoticon 
            </v-icon>
          </div>
        </v-form>
        <!-- <math-question
          :question="question"
          @enviar-click="onEnviarClick"
        >
        </math-question> -->
      </v-col>

    </v-row>
  </v-container>


</template>

<script>
  export default {
    name: 'MathQuestion',
    props: {
      question: Object,
    },

    data: () => ({
      result: null,
    }),
  }
</script>
