<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from "vue";

const props = defineProps(["chart_config", "activeChart", "series"]);

const chartOptions = ref({
	chart: {
		type: "bar",
		height: 350,
		toolbar: {
			show: false,
		},
	},
	plotOptions: {
		bar: {
			borderRadius: 0,
			horizontal: true,
			barHeight: "80%",
			isFunnel: true,
		},
	},
	dataLabels: {
		enabled: true,
		formatter: function (val, opt) {
			return opt.w.globals.labels[opt.dataPointIndex] + ":  " + val;
		},
		dropShadow: {
			enabled: true,
		},
	},
	xaxis: {
		categories: [
			"台北市每人每日生活用水量",
			"轄區每人每日生活用水量",
			"轄區每人每日家庭用水量",
			"台北市每人每日家庭用水量",
		],
	},
	legend: {
		show: false,
	},
	colors: [props.chart_config.color[0]],
	markers: {
		colors: ["#F44336", "#E91E63", "#9C27B0"],
	},
});
</script>

<template>
	<div v-if="activeChart === 'FunnelChart'">
		<apexchart
			type="bar"
			height="260px"
			:options="chartOptions"
			:series="series"
		></apexchart>
	</div>
</template>
