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
	chart: {
		height: 350,
		type: "scatter",
		zoom: {
			enabled: true,
			type: "xy",
		},
	},
	xaxis: {
		tickAmount: 10,
		labels: {
			formatter: function (val) {
				return parseFloat(val).toFixed(1);
			},
		},
	},
	yaxis: {
		tickAmount: 7,
	},
});

const selectedIndex = ref(null);
console.log({ props });
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
			:height="350"
			type="scatter"
			:options="chartOptions"
			:series="series"
			@dataPointSelection="handleDataSelection"
		></apexchart>
	</div>
</template>

