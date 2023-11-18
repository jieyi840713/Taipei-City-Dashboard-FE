<script setup>
import { ref, computed } from "vue";
import { useMapStore } from "../../store/mapStore";

const props = defineProps([
	"chart_config",
	"activeChart",
	"series",
	"map_config",
]);
const mapStore = useMapStore();

const chartOptions = ref({
        el: '#app',
        components: {
          apexchart: VueApexCharts,
        },
        data: {
          
          series: [{
            name: 'Product1',
            data: generateData(new Date('11 Feb 2017 GMT').getTime(), 20, {
              min: 10,
              max: 60
            })
          },
          {
            name: 'Product2',
            data: generateData(new Date('11 Feb 2017 GMT').getTime(), 20, {
              min: 10,
              max: 60
            })
          },
          {
            name: 'Product3',
            data: generateData(new Date('11 Feb 2017 GMT').getTime(), 20, {
              min: 10,
              max: 60
            })
          },
          {
            name: 'Product4',
            data: generateData(new Date('11 Feb 2017 GMT').getTime(), 20, {
              min: 10,
              max: 60
            })
          }],
          chartOptions: {
            chart: {
              height: 350,
              type: 'bubble',
            },
            dataLabels: {
              enabled: false
            },
            fill: {
              type: 'gradient',
            },
            title: {
              text: '3D Bubble Chart'
            },
            xaxis: {
              tickAmount: 12,
              type: 'datetime',
              labels: {
                  rotate: 0,
              }
            },
            yaxis: {
              max: 70
            },
            theme: {
              palette: 'palette2'
            }
          },
          
          
        },
        
      });

const chartHeight = computed(() => {
	return `${40 + props.series[0].data.length * 24}`;
});

const selectedIndex = ref(null);

function handleDataSelection(e, chartContext, config) {
	if (!props.chart_config.map_filter) {
		return;
	}
	if (config.dataPointIndex !== selectedIndex.value) {
		mapStore.addLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`,
			props.chart_config.map_filter[0],
			props.chart_config.map_filter[1][config.dataPointIndex]
		);
		selectedIndex.value = config.dataPointIndex;
	} else {
		mapStore.clearLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`
		);
		selectedIndex.value = null;
	}
}
</script>

<template>
	<div v-if="activeChart === 'BubbleChart'">
		<apexchart
			width="100%"
			:height="chartHeight"
			type="bubble"
			:options="chartOptions"
			:series="series"
			@dataPointSelection="handleDataSelection"
		></apexchart>
	</div>
</template>
