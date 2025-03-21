<template>
	<div v-for="(lineItem, index) in lineItems" class="forecastLine2">
		<h3>{{ LineItems.nameLine[index] }}</h3>
		<Line :key="index" :data="lineItem.data" :options="lineItem.options" />
	</div>
</template>

<script setup>
import { ref } from 'vue'
import {
	smoothedExponentialSeries,
	trendValue,
	seasonForYear,
	allMonths,
} from '../controllers/holtWintersController.vue'

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

// Регистрация компонентов Chart.js
ChartJS.register(
	CategoryScale,
	LinearScale,
	PointElement,
	LineElement,
	Title,
	Tooltip,
	Legend
)

// Определение LineItems
const LineItems = {
	nameLine: ['Сглаженный экспоненциальный ряд', 'Тренд', 'Сезонность'],
	dataLine: [smoothedExponentialSeries, trendValue, seasonForYear.slice(11)],
	colorLine: ['#7e01b8', '#ff8800', '#2ec700'],
}

// Создание массива lineItems с данными и опциями для каждого графика
const lineItems = ref(
	LineItems.nameLine.map((name, index) => ({
		data: {
			labels: allMonths,
			datasets: [
				{
					label: name,
					borderColor: LineItems.colorLine[index],
					backgroundColor: LineItems.colorLine[index],
					data: LineItems.dataLine[index],
				},
			],
		},
		options: {
			responsive: true,
			maintainAspectRatio: false,
			scales: {
				y: {
					type: 'linear',
					display: true,
					position: 'left',
					ticks: {
						display: false,
					},
					grid: {
						borderStyle: 'dashed',
						color: '#c9d1e8',
						lineWidth: 1,
					},
				},
				x: {
					type: 'category',
					ticks: {
						autoSkip: false,
						maxRotation: 90,
						minRotation: 90,
					},
					grid: {
						color: '#c9d1e8',
						lineWidth: 1,
					},
				},
			},
		},
	}))
)
</script>
