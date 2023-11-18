<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from "vue";

const props = defineProps(["chart_config", "activeChart", "series"]);

const chartOptions = ref({
	chart: {
		height: 450,
		type: "rangeBar",
		toolbar: {
			show: false,
		},
	},
	plotOptions: {
		bar: {
			horizontal: true,
			barHeight: "80%",
		},
	},
	xaxis: {
		type: "datetime",
	},
	stroke: {
		width: 1,
	},
	fill: {
		type: "solid",
		opacity: 0.6,
	},
	legend: {
		position: "top",
		horizontalAlign: "left",
	},

	// chart: {
	// 	stacked: true,
	// 	toolbar: {
	// 		show: false,
	// 		tools: {
	// 			zoom: false,
	// 		},
	// 	},
	// },
	// colors: props.chart_config.color,
	// dataLabels: {
	// 	enabled: false,
	// },
	// grid: {
	// 	show: false,
	// },
	// legend: {
	// 	show: props.series.length > 1 ? true : false,
	// },
	// markers: {
	// 	hover: {
	// 		size: 5,
	// 	},
	// 	size: 3,
	// 	strokeWidth: 0,
	// },
	// stroke: {
	// 	colors: props.chart_config.color,
	// 	curve: "smooth",
	// 	show: true,
	// 	width: 2,
	// },
	// tooltip: {
	// 	custom: function ({ series, seriesIndex, dataPointIndex, w }) {
	// 		// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
	// 		return (
	// 			'<div class="chart-tooltip">' +
	// 			"<h6>" +
	// 			`${parseTime(
	// 				w.config.series[seriesIndex].data[dataPointIndex].x
	// 			)}` +
	// 			` - ${w.globals.seriesNames[seriesIndex]}` +
	// 			"</h6>" +
	// 			"<span>" +
	// 			series[seriesIndex][dataPointIndex] +
	// 			` ${props.chart_config.unit}` +
	// 			"</span>" +
	// 			"</div>"
	// 		);
	// 	},
	// },
	// xaxis: {
	// 	axisBorder: {
	// 		color: "#555",
	// 		height: "0.8",
	// 	},
	// 	axisTicks: {
	// 		show: false,
	// 	},
	// 	crosshairs: {
	// 		show: false,
	// 	},
	// 	tooltip: {
	// 		enabled: false,
	// 	},
	// 	type: "datetime",
	// },
});

function parseTime(time) {
	return time.replace("T", " ").replace("+08:00", " ");
}
</script>

<template>
	<div v-if="activeChart === 'TimelineMultiSeriesChart'">
		<apexchart
			width="100%"
			type="rangeBar"
			height="350px"
			:options="chartOptions"
			:series="series"
		></apexchart>
	</div>
</template>
