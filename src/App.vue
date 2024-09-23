<template>
	<div>
		<h2>Прогноз продаж методом Хольта-Винтерса</h2>

		<div
			style="
				display: flex;
				align-items: center;
				justify-content: space-between;
				margin-bottom: 20px;
			"
		>
			<div>
				<label for="period">Выберите период:</label>
				<select v-model="selectedPeriod" id="period">
					<option value="day">День</option>
					<option value="month">Месяц</option>
				</select>
			</div>
			<div>
				<label for="periodCount">Количество периодов:</label>
				<input
					type="range"
					id="periodCount"
					v-model="periodCount"
					min="1"
					max="30"
				/>
				<span>{{ periodCount }}</span>
			</div>
		</div>

		<div style="margin-bottom: 20px">
			<Line :data="salesData" :options="options" />
		</div>

		<!-- Два столбца с таблицами -->
		<div
			style="display: flex; justify-content: space-between; margin-bottom: 20px"
		>
			<div style="flex: 1; margin-right: 10px">
				<h3>Таблица: Экспоненциально сглаженный ряд, Тренд, Сезонность</h3>
				<div class="table-container">
					<table>
						<thead>
							<tr>
								<th>Месяц</th>
								<th>День</th>
								<th>Lt = Экспоненциально сглаженный ряд</th>
								<th>Tt = Значение тренда</th>
								<th>St-s = Коэффициент сезонности предыдущего периода</th>
							</tr>
						</thead>
						<tbody>
							<tr
								v-for="(value, index) in salesData.datasets[0].data"
								:key="index"
							>
								<td>{{ getMonth(index) }}</td>
								<td>{{ getDay(index) }}</td>
								<td>{{ smoothedData.datasets[0].data[index] }}</td>
								<td>{{ trendData.datasets[0].data[index] }}</td>
								<td>{{ seasonalityData.datasets[0].data[index] }}</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>

			<div style="flex: 1; margin-left: 10px">
				<h3>Таблица: Прогноз по методу Хольта</h3>
				<div class="table-container">
					<table>
						<thead>
							<tr>
								<th>p = Номер периода</th>
								<th>Ŷt+p = Lt + p * Tt</th>
							</tr>
						</thead>
						<tbody>
							<tr v-for="p in periodCount" :key="p">
								<td>{{ p }}</td>
								<td>{{ forecastData[p - 1] }}</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>
		</div>

		<!-- Три графика в линию -->
		<div style="display: flex; justify-content: space-between">
			<div style="flex: 1; margin-right: 10px">
				<h3>Сглаженный экспоненциальный ряд</h3>
				<Line :data="smoothedData" :options="smoothedOptions" />
			</div>

			<div style="flex: 1; margin-right: 10px">
				<h3>Тренд</h3>
				<Line :data="trendData" :options="trendOptions" />
			</div>

			<div style="flex: 1">
				<h3>Сезонность</h3>
				<Line :data="seasonalityData" :options="seasonalityOptions" />
			</div>
		</div>
	</div>
</template>

<script>
import { ref, watch } from 'vue'
import { Line } from 'vue-chartjs'
import {
	Chart as ChartJS,
	Title,
	Tooltip,
	Legend,
	LineElement,
	PointElement,
	LinearScale,
	CategoryScale,
} from 'chart.js'

ChartJS.register(
	Title,
	Tooltip,
	Legend,
	LineElement,
	PointElement,
	LinearScale,
	CategoryScale
)

export default {
	components: { Line },
	setup() {
		const selectedPeriod = ref('day')
		const periodCount = ref(30)
		const forecastLength = 10

		const generateTestData = () => {
			const sales = []
			// Два месяца данных (60 дней)
			for (let i = 0; i < 60; i++) {
				sales.push(Math.floor(Math.random() * 200) + 50)
			}
			return sales
		}

		const salesData = ref({
			labels: [],
			datasets: [
				{
					label: 'Текущие продажи',
					data: [],
					borderColor: 'blue',
					fill: false,
				},
				{
					label: 'Прогнозируемые продажи',
					data: [],
					borderColor: 'red',
					fill: false,
				},
			],
		})

		const smoothedData = ref({
			labels: [],
			datasets: [
				{
					label: 'Сглаженный экспоненциальный ряд',
					data: [],
					borderColor: 'green',
					fill: false,
				},
			],
		})

		const trendData = ref({
			labels: [],
			datasets: [
				{
					label: 'Тренд',
					data: [],
					borderColor: 'orange',
					fill: false,
				},
			],
		})

		const seasonalityData = ref({
			labels: [],
			datasets: [
				{
					label: 'Сезонность',
					data: [],
					borderColor: 'purple',
					fill: false,
				},
			],
		})

		const forecastData = ref([])

		const options = {
			responsive: true,
			plugins: {
				legend: {
					display: true,
				},
			},
		}

		const updateSalesData = () => {
			const realSales = generateTestData()
			const totalDataPoints = realSales.length

			// Генерация меток в зависимости от выбранного периода
			salesData.value.labels = Array.from(
				{ length: totalDataPoints },
				(_, i) => {
					if (selectedPeriod.value === 'day') return `День ${i + 1}`
					if (selectedPeriod.value === 'month')
						return `Месяц ${Math.floor(i / 30) + 1}`
				}
			)

			salesData.value.datasets[0].data = realSales

			// Прогнозируемые продажи
			const lastSale = realSales[realSales.length - 1]
			const forecastedSales = Array.from(
				{ length: forecastLength },
				(_, index) => Math.floor(lastSale * Math.pow(1.1, index + 1))
			)
			salesData.value.datasets[1].data = [...realSales, ...forecastedSales]

			// Данные для других графиков
			smoothedData.value.labels = salesData.value.labels
			smoothedData.value.datasets[0].data = realSales.map(
				sale => sale * 0.8 + 20
			)

			trendData.value.labels = salesData.value.labels
			trendData.value.datasets[0].data = realSales.map(
				(sale, index) => sale + (index % 30) * 5
			)

			seasonalityData.value.labels = salesData.value.labels
			seasonalityData.value.datasets[0].data = realSales.map(
				sale => sale * Math.sin((Math.PI / 365) * (sale % 365))
			)

			// Прогноз по методу Хольта
			forecastData.value = Array.from({ length: forecastLength }, (_, p) => {
				const Lt = smoothedData.value.datasets[0].data[totalDataPoints - 1]
				const Tt = trendData.value.datasets[0].data[totalDataPoints - 1]
				return Lt + p * Tt
			})
		}

		const getMonth = index => Math.floor(index / 30) + 1
		const getDay = index => (index % 30) + 1

		watch([selectedPeriod, periodCount], updateSalesData)

		updateSalesData()

		return {
			salesData,
			options,
			smoothedData,
			trendData,
			seasonalityData,
			forecastData,
			selectedPeriod,
			periodCount,
			getMonth,
			getDay,
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
th {
}
h2,
h3 {
	text-align: center;
}
.table-container {
	max-height: 565px;
	overflow-y: auto;
	border: 1px solid #ccc;
	border-radius: 4px;
}
</style>
