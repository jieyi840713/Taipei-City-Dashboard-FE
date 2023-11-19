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
