<template>
	<table class="sales-table">
		<thead>
			<tr>
				<th>Дата</th>
				<th>Продажи (шт)</th>
				<th>Выручка (₽)</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="(item, index) in salesData" :key="index">
				<td>{{ item.date }}</td>
				<td>{{ item.sales }}</td>
				<td>{{ item.revenue }}</td>
			</tr>
		</tbody>
	</table>
</template>

<script>
import { defineComponent, ref } from 'vue'
import dayjs from 'dayjs'

export default defineComponent({
	setup() {
		const salesData = ref([
			{
				date: dayjs().subtract(6, 'days').format('DD/MM/YYYY'),
				sales: 100,
				revenue: 120000,
			},
			{
				date: dayjs().subtract(5, 'days').format('DD/MM/YYYY'),
				sales: 150,
				revenue: 180000,
			},
			{
				date: dayjs().subtract(4, 'days').format('DD/MM/YYYY'),
				sales: 200,
				revenue: 220000,
			},
			{
				date: dayjs().subtract(3, 'days').format('DD/MM/YYYY'),
				sales: 250,
				revenue: 270000,
			},
			{
				date: dayjs().subtract(2, 'days').format('DD/MM/YYYY'),
				sales: 300,
				revenue: 320000,
			},
			{
				date: dayjs().subtract(1, 'day').format('DD/MM/YYYY'),
				sales: 350,
				revenue: 370000,
			},
			{ date: dayjs().format('DD/MM/YYYY'), sales: 400, revenue: 420000 },
		])

		return {
			salesData,
		}
	},
})
</script>

<style>
.sales-table {
	width: 100%;
	border-collapse: collapse;
	margin: 20px 0;
}

.sales-table th,
.sales-table td {
	padding: 10px;
	border: 1px solid #ccc;
}

.sales-table th {
	background-color: #f8f8f8;
	text-align: left;
}

.sales-table td {
	text-align: center;
}
</style>
