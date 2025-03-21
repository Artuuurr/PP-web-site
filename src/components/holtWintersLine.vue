<template>
	<div>
		<div class="titelForBlock">Прогноз продаж методом Хольта-Винтерса</div>
		<div></div>
		<div class="forecastLine">
			<Line :data="data" :options="options" />
		</div>
	</div>
</template>

<script setup>
import { ref } from 'vue'
import { allAmounts, allMonths } from '../controllers/holtWintersController.vue'

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

const forecastPeriod = ref(2)

// Определение реактивных данных
const data = ref({
	labels: allMonths,
	datasets: [
		{
			label: 'Продажи',
			borderColor: 'rgb(34, 110, 223)',
			backgroundColor: 'rgba(34, 110, 223, 0.5)',
			data: allAmounts,
		},
		{
			label: 'Прогноз',
			data: [], // Здесь будут прогнозные данные
			borderColor: 'rgb(255, 87, 127)',
			backgroundColor: 'rgba(255, 87, 127, 0.5)',
		},
	],
})

// Опции для графика
const options = {
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
				borderDash: [5, 5],
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
				borderDash: [5, 5],
			},
		},
	},
}
</script>
