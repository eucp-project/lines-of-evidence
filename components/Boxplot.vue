<template>
  <div id="vis" />
</template>

<script>
import vegaEmbed from 'vega-embed'

export default {
  async mounted () {
    const spec = await fetch('data/chart.json').then(res => res.json())

    // Add generic event listener: https://stackoverflow.com/a/61782407
    vegaEmbed('#vis', spec).then((result) => {
      result.view.addEventListener('click', this.updateMap)
    }).catch(console.warn)
  },
  methods: {
    updateMap (event, item) {
      // When clicking outside the points, show the mean of the project
      const project = item.datum.project
      const dataset = item.datum.dataset
      const path = dataset === undefined ? project + '_mean' : project + '_' + dataset
      // console.log(project, dataset, path)
      this.$emit('updateMap', path)
    }
  }
}
</script>
