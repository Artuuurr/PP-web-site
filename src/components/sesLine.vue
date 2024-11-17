<script>
import { allAmounts, allMonths } from './holtWintersLine.vue'

import { seasonCoefficientForYear } from './seasonLine.vue'

const seasonCoefficientForFirstYear = 1 // изменяемый

const previousExponentiallySmoothedSeries = allAmounts[0] // изменяемый
const exponentiallySmoothedSeries = allAmounts[0 + 1] // изменяемый
const trendValue = [0] // изменяемый

const seriesSmoothingCoefficient = 0.5

export let smoothedExponentialSeries = [allAmounts[0]]

let calculatedsmoothedExponentialSeries =
	(seriesSmoothingCoefficient * exponentiallySmoothedSeries) /
		seasonCoefficientForFirstYear +
	(1 - seriesSmoothingCoefficient) *
		(previousExponentiallySmoothedSeries + trendValue)

// for (let index = 0; index < 12; index++) {
// 	calculatedsmoothedExponentialSeries[index] =
// 		(seriesSmoothingCoefficient * allAmounts[index + 1]) /
// 			seasonCoefficientForFirstYear[index] +
// 		(1 - seriesSmoothingCoefficient) * (smoothedExponentialSeries[index] + trendValue[index])
// }

trendValue.push()
smoothedExponentialSeries.push(calculatedsmoothedExponentialSeries)

function getsmoothedExponentialSeries(
	allAmounts,
	trendValue,
	seasonCoefficientForFirstYear
) {
	const seriesSmoothingCoefficient = 0.5

	array.forEach(element => {})
}
console.log('Экспоненциальный но сглаженный ряд:', smoothedExponentialSeries)

export const data = {
	labels: allMonths,
	datasets: [
		{
			label: 'Сглаженный экспоненциальный ряд',
			borderColor: '#7e01b8',
			backgroundColor: '#7e01b8',
			data: [smoothedExponentialSeries],
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
