<script>
import { allAmounts, allMonths } from '../controllers/holtWintersController.vue'

export const data = {
	labels: [],
	datasets: [
		{
			label: 'Продажи',
			borderColor: 'rgb(34, 110, 223)',
			backgroundColor: 'rgba(34, 110, 223, 0.5)',
			data: [],
		},
		{
			label: 'Прогноз',
			data: [],
			borderColor: 'rgb(255, 87, 127)',

			backgroundColor: 'rgba(255, 87, 127, 0.5)',
		},
	],
}

export const options = {
	responsive: true,
	maintainAspectRatio: false,
	lineSmooth: false,
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
