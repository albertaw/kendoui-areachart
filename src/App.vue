<template>
  <div id="app">
    <kendo-datasource
      ref="dataSource"
      :transport-read-url="'Spread.json'"
      :transport-read-data-type="'json'"
      :schema-parse="schemaParse">
    </kendo-datasource>
    <kendo-chart
      :data-source-ref="'dataSource'"
      :tooltip="tooltip"
      :series="series"
      :category-axis="categoryAxis"
      :value-axis="valueAxis"
      :theme="'blueopal'">
    </kendo-chart>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      tooltip: {
        visible: true,
        format: 'c6'
      },
      series: [{
        type: 'area',
        field: 'spread',
        categoryField: 'timestamp',
        name: 'Spread'
      }],
      categoryAxis: {
        labels: {
          visible: false
        },
        majorGridLines: {
          visible: false
        }
      },
      valueAxis: {
        labels: {
          format: 'c2'
        }
      }
    }
  },
  methods: {
    schemaParse: function(response) {
      return response.result.DASHUSD.map(function(arr) {
        return {
          timestamp: arr[0],
          bid: arr[1],
          ask: arr[2],
          spread: arr[2] - arr[1]
        }
      });
    }
  }
}
</script>

<style>
#app {
  margin-top: 60px;
}
</style>
