<script>
import { allAmounts, allMonths } from './holtWintersLine.vue'

export let seasonCoefficientForYear = []

for (let i = 0; i < 12; i++) {
	seasonCoefficientForYear.push(1)
}

console.log(
	'Коэффициент сезонности предыдущего периода:',
	seasonCoefficientForYear
)

export const data = {
	labels: allMonths,
	datasets: [
		{
			label: 'Сезонность',
			borderColor: '#2ec700',
			backgroundColor: '#2ec700',
			data: seasonCoefficientForYear,
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
