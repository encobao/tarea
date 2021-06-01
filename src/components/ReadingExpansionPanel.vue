<template>
    <v-expansion-panels 
        v-if="readingList.length > 1"
        accordion
    >
        <v-expansion-panel
            v-for="(reading, i) in readingList"
            :key="i"
        >
            <v-expansion-panel-header class="text-h5 secondary--text font-weight-bold">{{ reading.title }}</v-expansion-panel-header>
            <v-expansion-panel-content>
                <p class="text-h4 primary--text font-weight-bold text-center mb-8">
                    {{ reading.title }}
                </p>
                <p 
                v-for="para in reading.paras"
                :key="para.id"
                class="text-h5 text-justify"
                >
                <v-img
                    v-if="para.img"
                    :src="imgReadingPath + para.img"
                    width="100%"
                    class="mb-5"
                >
                </v-img>

                {{ para.text }}
                </p>
            </v-expansion-panel-content>
        </v-expansion-panel>
    </v-expansion-panels>

    <div 
        v-else
        class="px-5 mt-8"
    >
        <p class="text-h4 primary--text font-weight-bold text-center mb-8">
            {{ readingList[0].title }}
        </p>
        <p 
            v-for="(para, i) in readingList[0].paras"
            :key="i"
            class="text-h5 text-justify"
        >
            <v-img
                v-if="para.img"
                :src="imgReadingPath + para.img"
                width="100%"
                class="mb-5"
            >
            </v-img>

            {{ para.text }}
        </p>
    </div>

</template>

<script>
  export default {
    name: 'ReadingExpansionPanel',
    props: {
      readingList: Array,
    },

    data: () => ({
        imgReadingPath: process.env.VUE_APP_IMG_READING_FOLDER,
    }),
  }
</script>
