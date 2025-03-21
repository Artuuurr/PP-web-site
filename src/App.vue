<template>
	<div class="container">
		<div class="forecastLine">
			<HoltWintersLine />
		</div>
		<div class="downLine">
			<LineItem />
		</div>
	</div>
</template>
<script setup>
import LineItem from './components/LineItem.vue'
import HoltWintersLine from './components/HoltWintersLine.vue'
</script>
