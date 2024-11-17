<script>
import { sales } from './salesData.vue'

export let allAmounts = []
export let allMonths = []

function getSales(sales) {
	sales.forEach(oneYear => {
		oneYear.months.forEach(oneMonth => {
			allMonths.push(
				`${oneMonth.month
					.toString()
					.padStart(2, '0')}.${oneYear.salesYear.toString().slice(-2)}`
			)
			allAmounts.push(oneMonth.amount)
		})
	})
}

getSales(sales)

export const data = {
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
