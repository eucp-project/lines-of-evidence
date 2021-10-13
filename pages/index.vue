<template>
  <div class="flex flex-col overflow-auto place-content-center items-center w-screen h-full">
    <h1 class="m-4 text-3xl">
      EUCP WP5 - Demonstrator lines of evidence
    </h1>
    <div class="space-x-1 item-center">
      <!-- <Dropdown v-model="selectedProject" :options="projects" alttext="Choose a project." /> -->
      <!-- <Dropdown v-model="selectedModel" :options="models" alttext="Select a model." /> -->
      <Dropdown v-model="selectedSeason" :options="seasons" alttext="Select a season." />
      <Dropdown v-model="selectedRegion" :options="regions" alttext="Select a region." />
    </div>
    <div class="flex flex-row overflow-auto place-content-center w-screen h-full">
      <div class="w-1/2">
        <div
          class="bg-center bg-no-repeat bg-contain flex-grow h-full"
          :style="{backgroundImage: `url(${bgImage})`}"
        />
      </div>
      <div class="flex items-center w-1/2 h-full">
        <Boxplot @updateMap="updateMap($event)" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      projectModel: 'CMIP5_ACCESS1-0',
      selectedSeason: 'DJF',
      selectedRegion: 'Alpine',
      projects: {
        CMIP5: 'CMIP5',
        CMIP6: 'CMIP6',
        'cordex-cpm': 'cordex-cpm',
        CORDEX: 'CORDEX'
      },
      models: {
        'ACCESS1-0': 'ACCESS1-0',
        'bcc-csm1-1': 'bcc-csm1-1'
      },
      seasons: {
        DJF: 'Winter',
        JJA: 'Summer',
        MAM: 'Spring',
        OND: 'Autumn'
      },
      regions: {
        Alpine: 'Alpine',
        Alps_NE: 'Alps_NE',
        Alps_NW: 'Alps_NW',
        Alps_SE: 'Alps_SE',
        Alps_SW: 'Alps_SW',
        DE_S: 'DE_S',
        FRA_S: 'FRA_S'
      }
    }
  },
  computed: {
    bgImage () {
      const fallback = 'placeholder.png'
      try {
        return require('~/assets/plots_' + this.selectedRegion + '/plots/maps/main/' + this.selectedSeason + '/' + this.projectModel + '_map_' + this.selectedSeason + '.png')
      } catch (err) {
        return fallback
      }
    }
  },
  methods: {
    updateMap (newValue) {
      this.projectModel = newValue
    }
  }
}
</script>
