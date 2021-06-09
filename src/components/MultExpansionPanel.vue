<template>
    <v-expansion-panels 
        accordion
    >
        <v-expansion-panel
            v-for="(multChartImg, i) in multChartImgs"
            :key="i"
        >
            <v-expansion-panel-header class="text-h5 secondary--text font-weight-bold">
                {{ multChartImg.name }}
            </v-expansion-panel-header>
            <v-expansion-panel-content class="pt-4">
                <v-img
                    :src="imgChartsFolder + multChartImg.img"
                    width="100%"
                ></v-img>
            </v-expansion-panel-content>
        </v-expansion-panel>
    </v-expansion-panels>

</template>

<script>
  export default {
    name: 'MultExpansionPanel',
    props: {
      multChartImgs: Array,
    },

    data: () => ({
        imgChartsFolder: process.env.VUE_APP_IMG_CHARTS_FOLDER,
    }),

    methods: {
    }

  }
</script>
