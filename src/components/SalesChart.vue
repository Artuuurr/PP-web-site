<template>
	<div>
		<line-chart :chart-data="chartData" />
	</div>
</template>

<script>
import { defineComponent, ref } from 'vue'
import { Line } from 'vue-chartjs'
import { Chart, registerables } from 'chart.js'
import dayjs from 'dayjs'

Chart.register(...registerables)

export default defineComponent({
	components: {
		LineChart: Line,
	},
	setup() {
		const chartData = ref({
			labels: [
				dayjs().subtract(6, 'days').format('DD/MM'),
				dayjs().subtract(5, 'days').format('DD/MM'),
				dayjs().subtract(4, 'days').format('DD/MM'),
				dayjs().subtract(3, 'days').format('DD/MM'),
				dayjs().subtract(2, 'days').format('DD/MM'),
				dayjs().subtract(1, 'day').format('DD/MM'),
				dayjs().format('DD/MM'),
			],
			datasets: [
				{
					label: 'Продажи',
					backgroundColor: 'rgba(75, 192, 192, 0.2)',
					borderColor: 'rgba(75, 192, 192, 1)',
					data: [100, 150, 200, 250, 300, 350, 400],
				},
				{
					label: 'Выручка',
					backgroundColor: 'rgba(255, 99, 132, 0.2)',
					borderColor: 'rgba(255, 99, 132, 1)',
					data: [120, 180, 220, 270, 320, 370, 420],
				},
			],
		})

		return {
			chartData,
		}
	},
})
</script>
