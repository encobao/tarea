<template>
  <v-form>
    <div 
      v-if="!question.completed"
      class="d-flex"
    >
      <v-text-field 
        v-model="result"
        class="text-h4 pr-4"
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
        enviar
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

</template>

<script>
  export default {
    name: 'MyForm',
    props: {
      question: Object,
    },

    data: () => ({
      result: null,
    }),
  }
</script>
