<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from "vue";
import { useMapStore } from "../../store/mapStore";
import { useContentStore } from "../../store/contentStore";

const { BASE_URL } = import.meta.env;

const props = defineProps(["chart_config", "series", "map_config"]);
const mapStore = useMapStore();
const contentStore = useContentStore();

const selectedIndex = ref(null);

function handleDataSelection(index) {
	if (!props.chart_config.map_filter) {
		return;
	}
	if (index !== selectedIndex.value) {
		mapStore.addLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`,
			props.chart_config.map_filter[0],
			props.chart_config.map_filter[1][index]
		);
		selectedIndex.value = index;
	} else {
		mapStore.clearLayerFilter(
			`${props.map_config[0].index}-${props.map_config[0].type}`
		);
		selectedIndex.value = null;
	}
}
</script>

<template>
	<div class="maplegend">
		<GuageSemi />
		<div
			class="reportissue"
			:style="{ width: '100%' }"
			v-if="props.map_config[0].index === 'patrol_designate_place_path'"
		>
			<h3 :style="{ textWrap: 'nowrap' }">
				From (121.53617333558235, 25.115993829884932)
			</h3>
			<input class="reportissue-input" type="text" value="台北市立大學" />
			<h3 :style="{ textWrap: 'nowrap' }">
				To (121.5637902103862, 25.037654911446854)
			</h3>
			<input class="reportissue-input" type="text" value="台北市政府" />
		</div>
		<div class="maplegend-legend">
			<button
				v-for="(item, index) in series"
				:key="item.name"
				@click="handleDataSelection(index)"
				:class="{
					'maplegend-legend-item': true,
					'maplegend-filter':
						contentStore.currentDashboard.mode === '/mapview' &&
						props.chart_config.map_filter,
					'maplegend-selected':
						contentStore.currentDashboard.mode === '/mapview' &&
						selectedIndex === index,
				}"
			>
				<!-- Show different icons for different map types -->
				<div
					v-if="item.type !== 'symbol'"
					:style="{
						backgroundColor: `${item.color}`,
						height: item.type === 'line' ? '0.4rem' : '1rem',
						borderRadius: item.type === 'circle' ? '50%' : '2px',
					}"
				></div>
				<img v-else :src="`${BASE_URL}/images/map/${item.icon}.png`" />
				<!-- If there is a value attached, show the value -->
				<div v-if="item.value">
					<h5>{{ item.name }}</h5>
					<h6>{{ item.value }} {{ chart_config.unit }}</h6>
				</div>
				<div v-else>
					<h6>{{ item.name }}</h6>
				</div>
			</button>
		</div>
	</div>
</template>

<style scoped lang="scss">
.maplegend {
	width: 100%;
	height: 100%;
	display: flex;
	align-items: center;
	justify-content: center;
	margin-top: -1rem;
	overflow: visible;

	&-legend {
		width: 100%;
		display: grid;
		grid-template-columns: 1fr 1fr;
		column-gap: 0.5rem;
		row-gap: 0.5rem;
		overflow: visible;

		&-item {
			display: flex;
			align-items: center;
			padding: 5px 10px 5px 5px;
			border: 1px solid transparent;
			border-radius: 5px;
			transition: box-shadow 0.2s;
			cursor: auto;

			div:first-child,
			img {
				width: 1rem;
				margin-right: 0.75rem;
			}

			h5 {
				color: var(--color-complement-text);
				font-size: 0.75rem;
				text-align: left;
			}

			h6 {
				font-size: 1rem;
				font-weight: 400;
				text-align: left;
			}
		}
	}

	&-filter {
		border: 1px solid var(--color-border);
		cursor: pointer;

		&:hover {
			box-shadow: 0px 0px 5px black;
		}
	}

	&-selected {
		box-shadow: 0px 0px 5px black;
	}
}

.reportissue {
	width: 300px;
	display: flex;
	flex-direction: column;
	overflow: visible;

	h3 {
		margin: 0.5rem 0;
		font-size: var(--font-s);
		font-weight: 400;
	}

	label {
		position: relative;
		display: flex;
		align-items: center;
		font-size: var(--font-s);
		color: var(--color-complement-text);
		transition: color 0.2s;
		cursor: pointer;

		div {
			width: calc(var(--font-s) / 2);
			height: calc(var(--font-s) / 2);
			margin-right: 4px;
			padding: calc(var(--font-s) / 4);
			border-radius: 50%;
			border: 1px solid var(--color-border);
			transition: background-color 0.2s, border-color 0.2s;
		}
	}

	&-input {
		padding: 4px 6px;
		border: solid 1px var(--color-border);
		border-radius: 5px;
		background-color: transparent;
		font-size: var(--font-m);

		&:focus {
			outline: none;
			border: solid 1px var(--color-highlight);
		}
	}
}
</style>
