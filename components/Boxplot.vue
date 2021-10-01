<template>
  <div id="vis" />
</template>

<script>
import vegaEmbed from 'vega-embed'

export default {
  data () {
    return {
      spec: {},
      vegaLiteSpec: {
        $schema: 'https://vega.github.io/schema/vega-lite/v4.8.1.json',
        config: {
          view: {
            continuousHeight: 300,
            continuousWidth: 400
          }
        },
        data: {
          url: '/data/testdata.json'
        },
        encoding: {
          x: {
            field: 'age',
            type: 'ordinal'
          },
          y: {
            field: 'people',
            type: 'quantitative'
          }
        },
        mark: 'boxplot'
      }
    }
  },
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
