<script>
	import ScrollableDatePicker from '$lib/ScrollableDatePicker.svelte';
</script>

<h1>Welcome to your ScrollableDatePicker</h1>

<b> ScrollableDatePicker example: </b>
<div class="w-3/4">
	<ScrollableDatePicker
		selectedDate="2023-01-23"
		rowsShown={3}
		separatorWidth={4}
		rowBackgroundColor="#03FFFF"
	/>
</div>
