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
      console.log(item.datum)
      const path = item.datum.project + '_' + item.datum.dataset + '_map_' + 'DJF.png' // TODO use season_number
      alert(path)
    }
  }
}
</script>
