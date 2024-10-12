<script>
	import ScrollableDatePicker from '$lib/ScrollableDatePicker.svelte';

	$: selectedDate = '2012-12-11';
</script>

<h1>Welcome to your ScrollableDatePicker</h1>

<b> ScrollableDatePicker example: </b>
<div class="w-3/4">
	<ScrollableDatePicker {selectedDate} rowsShown={5} separatorWidth={4} startFrom="2014-10-11" />
</div>
