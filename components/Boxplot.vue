<template>
  <div id="vis" />
</template>

<script>
import vegaEmbed from 'vega-embed'

export default {
  props: {
    season: {
      type: String,
      required: true
    },
    region: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      view: {}
    }
  },
  watch: {
    // Update view whenever season or region changes
    season: 'updateView',
    region: 'updateView'
  },
  async mounted () {
    const spec = await fetch('chart.json').then(res => res.json())

    // Instead of a URL, use a named dataset that we can update through the view API
    delete spec.data.url
    spec.data.name = 'myData'

    this.view = await vegaEmbed('#vis', spec).then(result => result.view).catch(console.warn)
    this.updateView()
    this.view.addEventListener('click', this.updateMap)
  },
  methods: {
    updateMap (event, item) {
      let project = item.datum.project
      let dataset = item.datum.dataset
      const ensemble = item.datum.ensemble

      // Custom paths for UKCP and CORDEX
      if (project === 'UKCP-GCM') {
        project = 'UKCP18%20land-gcm'
        dataset = String(ensemble).padStart(2, '0')
      } else if (project === 'UKCP-RCM') {
        project = 'UKCP18%20land-rcm'
        dataset = String(ensemble).padStart(2, '0')
      } else if (project === 'CORDEX-EUR11') {
        console.log(project, dataset, ensemble)
      }

      // When clicking outside the points, show the mean
      if (dataset === undefined) {
        dataset = 'mean'
      }
      if (dataset === 'undefined') {
        dataset = 'mean'
      }

      // Trigger update
      const path = `${project}_${dataset}`
      this.$emit('updateMap', path)
    },
    async updateView () {
      // Update source data and re-render the figure
      const path = 'data/' + this.region + '_' + this.season + '.json'
      const data = await fetch(path).then(res => res.json())

      this.view.remove('myData', d => true).run() // remove all previous entries
      this.view.insert('myData', data).run()
      this.view.resize().run()
    }
  }
}
</script>
