<template>
    <v-expansion-panels 
        accordion
    >
        <v-expansion-panel
            v-for="operation in operationList"
            :key="operation.id"
        >
            <v-expansion-panel-header class="text-h5 secondary--text font-weight-bold">
                {{ operation.type }} 
                <span class="text-h6 ml-3"> 
                    ( 
                        {{ numberOfCompleted(operation.id) }} / {{ operation.list.length }}
                    ) 
                    <v-icon
                        v-if="numberOfCompleted(operation.id) == operation.list.length"
                        class="pl-3"
                        color="primary"
                    >
                        mdi-check-bold
                    </v-icon>
                </span>
            </v-expansion-panel-header>
            <v-expansion-panel-content class="pt-4">
                <math-question
                    v-for="formula in operation.list"
                    :key="formula.id"
                    :question="formula"
                    :operationId="operation.id"
                    @enviar-click="onEnviarClick"
                >
                </math-question>
            </v-expansion-panel-content>
        </v-expansion-panel>
    </v-expansion-panels>

</template>

<script>
import MathQuestion from './MathQuestion.vue';

  export default {
    name: 'MathExpansionPanel',
    props: {
      operationList: Array,
    },

    components: {
        MathQuestion,
    },

    data: () => ({
        //imgReadingPath: process.env.VUE_APP_IMG_READING_PATH,
    }),

    methods: {
        onEnviarClick(result, id, operationId) {
            this.$emit('enviar-click', result, id, operationId)
        },

        numberOfCompleted ( opId ) {
            let index = this.operationList.findIndex( val => val.id == opId )
            return this.operationList[index].list.reduce((total, v) => (v.completed === true ? total + 1 : total), 0)
        },
    }

  }
</script>
