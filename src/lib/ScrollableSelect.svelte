<script lang="ts">
	import { onMount } from 'svelte';
	import './scrollable-select.css';

	type Item = {
		name: string;
		value: string | number;
	};

	export let rowsShown: 1 | 3 | 5 | 7 = 3;
	export let items: Item[] = [
		{
			name: 'Item 1',
			value: 'item1'
		}
	];
	export let specialClass = 'special-style-class-here';
	export let maxWidth = '100%';

	let itemPadding = 1;
	let scrollContainer: HTMLUListElement;

	export let value: string | number = 'item1';

	const handleScroll = () => {
		const scrollTop = scrollContainer.scrollTop ?? 0;
		const heightOfEachItem = scrollContainer.children[0].clientHeight;
		const index = Math.round(scrollTop / heightOfEachItem);
		value = items[index].value;
		highlightTarget(index);
	};

	onMount(async () => {
		itemPadding = (rowsShown - 1) / 2;
		let targetIndex = items.findIndex((item) => item.value === value);
		if (targetIndex === -1) {
			targetIndex = items.length - 1;
			value = items[targetIndex].value;
		}

		// sleep for 100ms to allow the scroll to finish
		await new Promise((r) => setTimeout(r, 100));
		scrollContainer.scrollTo({
			top: targetIndex * scrollContainer.children[0].clientHeight,
			behavior: 'smooth'
		});

		highlightTarget(targetIndex);
	});

	function highlightTarget(index: number) {
		for (const element of scrollContainer.children) {
			element.classList.remove('font-medium');
			element.classList.add('font-light');
			element.classList.remove('scale-[2]');
			element.classList.remove('scale-150');
			element.classList.remove('scale-125');

			element.classList.remove('opacity-100');
			element.classList.remove('opacity-80');
			element.classList.remove('opacity-60');
			element.classList.add('opacity-40');
			element.classList.remove(specialClass);
		}
		scrollContainer.children[index + itemPadding].classList.remove('font-light');
		scrollContainer.children[index + itemPadding].classList.add('font-medium');
		scrollContainer.children[index + itemPadding].classList.add('scale-[2]');
		scrollContainer.children[index + itemPadding].classList.remove('opacity-40');
		scrollContainer.children[index + itemPadding].classList.add('opacity-100');
		scrollContainer.children[index + itemPadding].classList.add(specialClass);

		if (rowsShown >= 3) {
			scrollContainer.children[index + itemPadding - 1].classList.add('scale-150');
			scrollContainer.children[index + itemPadding + 1].classList.add('scale-150');
			scrollContainer.children[index + itemPadding - 1].classList.remove('opacity-40');
			scrollContainer.children[index + itemPadding + 1].classList.remove('opacity-40');
			scrollContainer.children[index + itemPadding - 1].classList.add('opacity-80');
			scrollContainer.children[index + itemPadding + 1].classList.add('opacity-80');
		}

		if (rowsShown >= 5) {
			scrollContainer.children[index + itemPadding - 2].classList.add('scale-125');
			scrollContainer.children[index + itemPadding + 2].classList.add('scale-125');
			scrollContainer.children[index + itemPadding - 2].classList.remove('opacity-40');
			scrollContainer.children[index + itemPadding + 2].classList.remove('opacity-40');
			scrollContainer.children[index + itemPadding - 2].classList.add('opacity-60');
			scrollContainer.children[index + itemPadding + 2].classList.add('opacity-60');
		}
	}
</script>

<div class="flex w-full overflow-y-auto text-center" style="max-width: {maxWidth};">
	<div
		class="hide-scrollbar flex flex-auto flex-col overflow-y-hidden duration-300 ease-in"
		style="height: {rowsShown * 4}rem"
	>
		<ul
			bind:this={scrollContainer}
			class="no-scrollbar overflow-x-hidden snap-y snap-mandatory overflow-scroll z-10"
			on:scroll={handleScroll}
		>
			<!--  eslint-disable @typescript-eslint/no-unused-vars -->
			{#each { length: itemPadding } as _}
				<li
					class="h-16 snap-center text-xs snap-normal content-center text-center font-light transition-all"
				>
					&nbsp;
				</li>
			{/each}
			{#each items as item}
				<li
					class="h-16 opacity-40 text-xs snap-center snap-normal content-center text-center font-light transition-all"
				>
					{item.name}
				</li>
			{/each}
			<!--  eslint-disable @typescript-eslint/no-unused-vars -->
			{#each { length: itemPadding } as _}
				<li
					class="h-16 snap-center text-xs snap-normal content-center text-center font-light transition-all"
				>
					&nbsp;
				</li>
			{/each}
		</ul>
	</div>
</div>
