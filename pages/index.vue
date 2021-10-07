<template>
  <div class="flex flex-col overflow-auto place-content-center items-center w-screen h-full">
    <h1 class="m-4 text-3xl">
      EUCP WP5 - Demonstrator lines of evidence
    </h1>
    <div class="space-x-1">
      <Dropdown v-model="selectedProject" :options="projects" alttext="Choose a project." />
      <Dropdown v-model="selectedModel" :options="models" alttext="Select a model." />
      <Dropdown v-model="selectedSeason" :options="seasons" alttext="Select a season." />
      <Dropdown v-model="selectedRegion" :options="regions" alttext="Select a region." />
    </div>
    <div
      class="bg-center bg-no-repeat bg-contain flex-grow w-full"
      :style="{backgroundImage: `url(${bgImage})`}"
    />
    <Boxplot />
  </div>
</template>

<script>
export default {
  data () {
    return {
      selectedProject: 'CMIP5',
      selectedModel: 'ACCESS1-0',
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
        return require('~/assets/plots_' + this.selectedRegion + '/plots/maps/main/' + this.selectedSeason + '/' + this.selectedProject + '_' + this.selectedModel + '_map_' + this.selectedSeason + '.png')
      } catch (err) {
        return fallback
      }
    }
  }
}
</script>
