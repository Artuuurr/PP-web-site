<script>
import { ref, watch } from 'vue'
import { sales } from '../salesData.vue'

// Filter sales
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

let smoothedExponentialSeries = [allAmounts[0]]
let trendValue = [0]
let seasonCoefficientForYear = []

//Калькулятор smoothedExponentialSeries
function calculateSmoothedExponentialSeries() {
	const seriesSmoothingCoefficient = 0.5
	for (let index = 0; index < 12; index++) {
		let calculatedValue =
			(seriesSmoothingCoefficient * allAmounts[index + 1]) /
				seasonCoefficientForYear[index] +
			(1 - seriesSmoothingCoefficient) *
				(smoothedExponentialSeries[index] + trendValue[index])
		smoothedExponentialSeries.push(calculatedValue)
	}
}

// Калькулятор trendValue
function calculateTrendValue() {
	const trendSmoothingCoefficient = 0.9
	for (let index = 0; index < 12; index++) {
		let calculatedValue =
			trendSmoothingCoefficient *
				(smoothedExponentialSeries[index + 1] -
					smoothedExponentialSeries[index]) +
			(1 - trendSmoothingCoefficient) * trendValue[index]
		trendValue.push(calculatedValue)
	}
}

// Калькулятор seasonCoefficientForYear
function calculateSeasonCoefficientForYear() {
	for (let i = 0; i < 12; i++) {
		seasonCoefficientForYear.push(1)
	}
}

calculateSeasonCoefficientForYear()
calculateSmoothedExponentialSeries()
calculateTrendValue()

console.log('Экспоненциальный но сглаженный ряд:', smoothedExponentialSeries)
console.log('Тренд:', trendValue)
console.log(
	'Коэффициент сезонности предыдущего периода:',
	seasonCoefficientForYear
)
</script>
