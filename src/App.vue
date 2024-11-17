<template>
	<div class="container">
		<div>
			<div class="titelForBlock">Прогноз продаж методом Хольта-Винтерса</div>
			<div class="forecastLine">
				<Line :data="holtWintersData" :options="holtWintersOptions" />
			</div>
			<div class="downLine">
				<div class="forecastLine2">
					<h3>Сглаженный экспоненциальный ряд</h3>
					<Line :data="sesData" :options="sesOptions" />
				</div>
				<div class="forecastLine2">
					<h3>Тренд</h3>
					<Line :data="trendData" :options="trendOptions" />
				</div>
				<div class="forecastLine2">
					<h3>Сезонность</h3>
					<Line :data="sesonData" :options="sesonOptions" />
				</div>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import {
	Chart as ChartJS,
	CategoryScale,
	LinearScale,
	PointElement,
	LineElement,
	Title,
	Tooltip,
	Legend,
} from 'chart.js'
import { Line } from 'vue-chartjs'
import {
	data as holtWintersData,
	options as holtWintersOptions,
} from './components/holtWintersLine.vue'

import {
	data as sesData,
	options as sesOptions,
} from './components/sesLine.vue'
import {
	data as sesonData,
	options as sesonOptions,
} from './components/seasonLine.vue'
import {
	data as trendData,
	options as trendOptions,
} from './components/trendLine.vue'

ChartJS.register(
	CategoryScale,
	LinearScale,
	PointElement,
	LineElement,
	Title,
	Tooltip,
	Legend
)

export default {
	name: 'App',
	components: {
		Line,
	},

	data() {
		return {
			holtWintersData,
			holtWintersOptions,
			sesData,
			sesOptions,
			sesonData,
			sesonOptions,
			trendData,
			trendOptions,
		}
	},
}
</script>
<style>
table {
	width: 100%;
	border-collapse: collapse;
}
th,
td {
	border: 1px solid #ccc;
	padding: 10px;
	text-align: left;
}
.table-container {
	max-height: 565px;
	overflow-y: auto;
	border: 1px solid #ccc;
	border-radius: 4px;
}
</style>
