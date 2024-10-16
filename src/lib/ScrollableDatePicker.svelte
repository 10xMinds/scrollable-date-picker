<script lang="ts">
	import ScrollableSelect from './ScrollableSelect.svelte';
	export let selectedDate = '1999-01-31';
	export let rowsShown: 1 | 3 | 5 | 7 = 3;
	export let separatorWidth = 4;
	export let backgroundClass = 'rounded-full';
	export let highlightClass = 'text-xxl';
	export let startFrom: string | undefined = undefined;
	export let endAt: string | undefined = undefined;

	let currentDay = parseInt(selectedDate.slice(8));
	let currentMonth = parseInt(selectedDate.slice(5, 7));
	let currentYear = parseInt(selectedDate.slice(0, 4));

	const getMinYear = () => {
		if (startFrom) {
			const startYear = parseInt(startFrom.split('-')[0]);
			if (isNaN(startYear)) {
				throw Error('Invalid `startFrom');
			}
			return startYear;
		}
		const year = new Date().getFullYear() - 100;
		return year;
	};
	const getMaxYear = () => {
		if (endAt) {
			const endYear = parseInt(endAt.split('-')[0]);
			if (isNaN(endYear)) {
				throw Error('Invalid `endAt`');
			}
			return endYear;
		}
		return new Date().getFullYear() + 100;
	};

	let minYear = getMinYear();
	let maxYear = getMaxYear();

	const allMonths = [
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

	const getYears = () => {
		return Array.from({ length: maxYear - minYear + 1 }, (_, i) => {
			return { name: `${minYear + i}`, value: minYear + i };
		});
	};

	const getMonths = (currentYear: number) => {
		let start = 1;
		let end = 12;
		if (currentYear === maxYear && endAt !== undefined) {
			end = parseInt(endAt.split('-')[1]);
			if (isNaN(end)) {
				throw Error('Invalid `endAt');
			}
		}
		if (currentYear === minYear && startFrom !== undefined) {
			start = parseInt(startFrom.split('-')[1]);
			if (isNaN(start)) {
				throw Error('Invalid `startFrom`');
			}
		}
		return allMonths.filter(({ value }) => value >= start && value <= end);
	};

	const getDays = (currentYear: number, currentMonth: number) => {
		let start = 1;
		let end = new Date(currentYear, currentMonth, 0).getDate();
		if (currentYear === maxYear && endAt !== undefined) {
			const maxMonth = parseInt(endAt.split('-')[1]);
			if (isNaN(maxMonth)) {
				throw Error('Invalid `endAt`');
			}
			if (maxMonth === currentMonth) {
				end = parseInt(endAt.split('-')[2]);
				if (isNaN(end)) {
					throw Error('Invalid endAt');
				}
			}
		}
		if (currentYear === minYear && startFrom !== undefined) {
			const minMonth = parseInt(startFrom.split('-')[1]);
			if (isNaN(minMonth)) {
				throw Error('Invalid `startFrom`');
			}
			if (minMonth === currentMonth) {
				start = parseInt(startFrom.split('-')[2]);
				if (isNaN(start)) {
					throw Error('Invalid `startFrom`');
				}
			}
		}
		return Array.from({ length: end - start + 1 }, (_, i) => ({
			name: `${start + i}`,
			value: start + i
		}));
	};

	const years = getYears();
	$: months = getMonths(currentYear);
	$: days = getDays(currentYear, currentMonth);
	$: selectedDate = `${currentYear}-${currentMonth.toString().padStart(2, '0')}-${currentDay.toString().padStart(2, '0')}`;
</script>

<div class="relative parent">
	<div class="z-50 flex w-full justify-center overflow-y-auto text-center">
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

<style>
	.parent {
		width: 100%;
	}
	@media (min-width: 640px) {
		.parent {
			width: 50%;
		}
	}
</style>
