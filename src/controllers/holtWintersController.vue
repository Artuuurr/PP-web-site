<script>
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

export let smoothedExponentialSeries = [allAmounts[0]]
export let trendValue = [0]
export let seasonForYear = []

// Калькулятор seasonForYear
function calculateSeasonForYear1() {
	for (let i = 0; i < 23; i++) {
		seasonForYear.push(1)
	}
}

function calculateSeasonForYear2(index) {
	const seasonalSmoothingCoefficient = 0.5
	let calculatedSeasonForYear =
		seasonalSmoothingCoefficient *
			(allAmounts[index] / smoothedExponentialSeries[index]) +
		(1 - seasonalSmoothingCoefficient) * seasonForYear[index]
	seasonForYear.push(Math.round(calculatedSeasonForYear * 100) / 100)
	// console.log(
	// 	allAmounts[index],
	// 	smoothedExponentialSeries[index],
	// 	seasonForYear[index]
	// )
}

//Калькулятор smoothedExponentialSeries
function calculateSmoothedExponentialSeries(index) {
	const seriesSmoothingCoefficient = 0.5

	let calculatedSmoothedExponentialSeries =
		(seriesSmoothingCoefficient * allAmounts[index + 1]) /
			seasonForYear[index] +
		(1 - seriesSmoothingCoefficient) *
			(smoothedExponentialSeries[index] + trendValue[index])
	smoothedExponentialSeries.push(
		Math.round(calculatedSmoothedExponentialSeries)
	)
	// console.log(
	// 	'Номер:',
	// 	index,
	// 	allAmounts[index + 1],
	// 	seasonForYear[index],
	// 	smoothedExponentialSeries[index],
	// 	trendValue[index],
	// 	'Ответ:',
	// 	calculatedSmoothedExponentialSeries
	// )
}

// Калькулятор trendValue
function calculateTrendValue(index) {
	const trendSmoothingCoefficient = 0.9
	let calculatedValue =
		trendSmoothingCoefficient *
			(smoothedExponentialSeries[index + 1] -
				smoothedExponentialSeries[index]) +
		(1 - trendSmoothingCoefficient) * trendValue[index]
	trendValue.push(Math.round(calculatedValue))
}

calculateSeasonForYear1()
for (let i = 0; i < 23; i++) {
	calculateSmoothedExponentialSeries(i)
	calculateTrendValue(i)
}
for (let i = 12; i < 24; i++) {
	calculateSeasonForYear2(i)
}
for (let i = 23; i < 35; i++) {
	calculateSmoothedExponentialSeries(i)
	calculateTrendValue(i)
}
for (let i = 24; i < 36; i++) {
	calculateSeasonForYear2(i)
}
for (let i = 35; i < 46; i++) {
	calculateSmoothedExponentialSeries(i)
	calculateTrendValue(i)
}
for (let i = 36; i < 37; i++) {
	calculateSeasonForYear2(i)
}
for (let i = 46; i < 48; i++) {
	calculateSmoothedExponentialSeries(i)
	calculateTrendValue(i)
}
for (let i = 37; i < 39; i++) {
	calculateSeasonForYear2(i)
}
for (let i = 48; i < 49; i++) {
	calculateSmoothedExponentialSeries(i)
	calculateTrendValue(i)
}
for (let i = 39; i < 50; i++) {
	calculateSeasonForYear2(i)
}

export let holtWinters = []
let forecastPeriodNumber = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
const seasonForYearForHoltWinters = seasonForYear.slice(0, -2).slice(-10)
let lastElementSmoothedExponentialSeriesForHoltWinters =
	smoothedExponentialSeries[smoothedExponentialSeries.length - 1]
let lastElementTrendValueForHoltWinters = trendValue[trendValue.length - 1]

const holtWintersValue = []

function calculateHoltWintersValue(i) {
	let calculatedValue =
		(lastElementSmoothedExponentialSeriesForHoltWinters +
			lastElementTrendValueForHoltWinters * forecastPeriodNumber[i]) *
		seasonForYearForHoltWinters[i]
	holtWintersValue.push(Math.round(calculatedValue))
}

for (let i = 0; i < forecastPeriodNumber.length; i++) {
	calculateHoltWintersValue(i)
}

console.log(holtWintersValue)

console.log('Продажи:', allAmounts)
console.log('Экспоненциальный но сглаженный ряд:', smoothedExponentialSeries)
console.log('Тренд:', trendValue)
console.log('Коэффициент сезонности предыдущего периода:', seasonForYear)
</script>
