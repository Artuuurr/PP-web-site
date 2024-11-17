<script>
import { allAmounts, allMonths } from './holtWintersLine.vue'
import { smoothedExponentialSeries } from './sesLine.vue'

let previousExponentiallySmoothedSeries = allAmounts[0] // изменяемый
let trendSmoothingCoefficient = 0.9 // не изменяемый
let previousTrendValue = null // изменяемый

export let trendValue =
	trendSmoothingCoefficient *
		(smoothedExponentialSeries[1] - previousExponentiallySmoothedSeries) +
	(1 - trendSmoothingCoefficient) * previousTrendValue

console.log('Тренд:', trendValue)

export const data = {
	labels: allMonths,
	datasets: [
		{
			label: 'Тренд',
			borderColor: '#ff8800',
			backgroundColor: '#ff8800',
			data: [40, 39, 10, 40, 39, 80, 40],
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
