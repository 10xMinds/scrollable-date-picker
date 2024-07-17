<script lang="ts">
	import ScrollableSelect from './ScrollableSelect.svelte';
	export let selectedDate = '1999-01-31';
	export let rowsShown: 1 | 3 | 5 | 7 = 3;
	export let separatorWidth = 4;
	export let backgroundClass = 'rounded-full';
	export let highlightClass = 'text-xxl';

	let currentDay = parseInt(selectedDate.slice(8));
	let currentMonth = parseInt(selectedDate.slice(5, 7));
	let currentYear = parseInt(selectedDate.slice(0, 4));

	const years = Array.from({ length: 200 }, (_, i) => {
		return { name: `${i + 1910}`, value: i + 1910 };
	});

	const months = [
		{ name: 'January', value: 1 },
		{ name: 'February', value: 2 },
		{ name: 'March', value: 3 },
		{ name: 'April', value: 4 },
		{ name: 'May', value: 5 },
		{ name: 'June', value: 6 },
		{ name: 'July', value: 7 },
		{ name: 'August', value: 8 },
		{ name: 'September', value: 9 },
		{ name: 'October', value: 10 },
		{ name: 'November', value: 11 },
		{ name: 'December', value: 12 }
	];
	let days = Array.from({ length: 31 }, (_, i) => {
		return { name: `${i + 1}`, value: i + 1 };
	});

	$: days = getDaysInMonth(currentMonth, currentYear);

	function getDaysInMonth(currentMonth: number, currentYear: number) {
		const daysCount = new Date(currentYear, currentMonth, 0).getDate();
		return Array.from({ length: daysCount }, (_, i) => {
			return { name: `${i + 1}`, value: i + 1 };
		});
	}

	$: {
		selectedDate = `${currentYear}-${currentMonth.toString().padStart(2, '0')}-${currentDay.toString().padStart(2, '0')}`;
	}
</script>

<div class="relative w-full">
	<div class="z-50 flex w-full overflow-y-auto text-center">
		<ScrollableSelect
			maxWidth="10rem"
			specialClass={highlightClass}
			items={months}
			{rowsShown}
			bind:value={currentMonth}
		/>
		<div
			style="background: #000; height: 2rem; margin-top: {((rowsShown - 1) / 2) * 4 +
				1}rem; width: {separatorWidth}px"
		>
			<div class="h-full w-full"></div>
		</div>
		<ScrollableSelect
			maxWidth="4rem"
			specialClass={highlightClass}
			items={days}
			{rowsShown}
			bind:value={currentDay}
		/>
		<div
			style="background: #000; height: 2rem; margin-top: {((rowsShown - 1) / 2) * 4 +
				1}rem; width: {separatorWidth}px"
		>
			<div class="h-full w-full"></div>
		</div>
		<ScrollableSelect
			maxWidth="6rem"
			specialClass={highlightClass}
			items={years}
			{rowsShown}
			bind:value={currentYear}
		/>
	</div>
	<div
		class="absolute -z-10 flex w-full {backgroundClass}"
		style="height: 4rem; bottom: {((rowsShown - 1) / 2) * 4}rem"
	></div>
</div>
