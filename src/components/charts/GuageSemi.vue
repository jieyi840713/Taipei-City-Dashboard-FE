<!-- Developed by Taipei Urban Intelligence Center 2023 -->

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

// Guage charts in apexcharts uses a slightly different data format from other chart types
// As such, the following parsing function are required
const parseSeries = computed(() => {
	let output = {};
	let parsedSeries = [];
	let parsedTooltip = [];
	for (let i = 0; i < props.series[0].data.length; i++) {
		let total = props.series[0].data[i] + props.series[1].data[i];
		parsedSeries.push(Math.round((props.series[0].data[i] / total) * 100));
		parsedTooltip.push(`${props.series[0].data[i]} / ${total}`);
	}
	output.series = parsedSeries;
	output.tooltipText = parsedTooltip;
	return output;
});

// chartOptions needs to be in the bottom since it uses computed data
const chartOptions = ref({
	chart: {
		type: "radialBar",
		offsetY: -20,
		sparkline: {
			enabled: true,
		},
	},
	plotOptions: {
		radialBar: {
			startAngle: -90,
			endAngle: 90,
			track: {
				background: "#e7e7e7",
				strokeWidth: "97%",
				margin: 5, // margin is in pixels
				dropShadow: {
					enabled: true,
					top: 2,
					left: 0,
					color: "#999",
					opacity: 1,
					blur: 2,
				},
			},
			dataLabels: {
				name: {
					show: false,
				},
				value: {
					offsetY: -2,
					fontSize: "22px",
				},
			},
		},
	},
	grid: {
		padding: {
			top: -10,
		},
	},
	fill: {
		type: "gradient",
		gradient: {
			shade: "light",
			shadeIntensity: 0.4,
			inverseColors: false,
			opacityFrom: 1,
			opacityTo: 1,
			stops: [0, 50, 53, 91],
		},
	},
	labels: ["Average Results"],
});

const selectedIndex = ref(null);

function handleDataSelection(e, chartContext, config) {
	if (!props.chart_config.map_filter) {
		return;
	}
	if (config.seriesIndex !== selectedIndex.value) {
		mapStore.addLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`,
			props.chart_config.map_filter[0],
			props.chart_config.map_filter[1][config.seriesIndex]
		);
		selectedIndex.value = config.seriesIndex;
	} else {
		mapStore.clearLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`
		);
		selectedIndex.value = null;
	}
}
</script>

<template>
	<div v-if="activeChart === 'GuageSemi'">
		<apexchart
			width="80%"
			height="300px"
			type="radialBar"
			:options="chartOptions"
			:series="parseSeries.series"
			@dataPointSelection="handleDataSelection"
		>
		</apexchart>
	</div>
</template>
