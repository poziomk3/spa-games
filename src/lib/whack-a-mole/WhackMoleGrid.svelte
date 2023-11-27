<script lang="ts">
	import WhackMoleField from './WhackMoleField.svelte';
	import { createEventDispatcher } from 'svelte';

	export let numberToRender: number;
	export let isRunning: boolean;
	export let intervalDuration: number;
	export let fieldNumber: number;
	export let produced: number;

	$: grid = Array(fieldNumber * fieldNumber);
	$: if (isRunning) setTimeout(updateGrid, 1000);
	$: if (!isRunning) {
		clearInterval(inverval);
		mole = -1;
	}

	let mole = -1;
	let dispatcher = createEventDispatcher();
	let inverval: NodeJS.Timeout;

	function updateGrid() {
		if (numberToRender - produced == 0) {
			mole = -1;
			return;
		}
		let found = false;
		while (!found) {
			let newMole = Math.floor(Math.random() * fieldNumber * fieldNumber);
			if (newMole != mole) {
				found = true;
				mole = newMole;
				dispatcher('produced');
			}
		}
		clearInterval(inverval);
		inverval = setInterval(() => {
			dispatcher('passed');
			updateGrid();
		}, intervalDuration);
	}

	function onWhack(event: any) {
		if (event.detail.isMole) {
			updateGrid();
			dispatcher('hit');
		} else {
			dispatcher('miss');
		}
	}
</script>

<div class="text-center pb-3 text-[2rem]">{numberToRender - produced} moles left!</div>
<div
	style="--fieldNumber:{fieldNumber}"
	class="w-full lg:flex-1 lg:w-auto  relative grid column-number aspect-square min-h-0  mx-auto gap-[0.5rem] w"
>
	{#if !isRunning}
		<div class="absolute w-full h-full bg-black/80 min-h-0"></div>{/if}
	{#each grid as cell, index}
		<WhackMoleField isMole={index == mole} on:whack={onWhack} />
	{/each}
</div>

<style lang="scss">
	.column-number {
		grid-template-columns: repeat(var(--fieldNumber), 1fr);
	}
</style>
