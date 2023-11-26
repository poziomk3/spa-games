<script lang="ts">
	import { writable } from 'svelte/store';
	import The2k48Grid from './The2k48Grid.svelte';
	let gameScore = writable(0);
</script>

<div class="h-full flex flex-col w-full justify-center text-center md:items-center items-stretch">
	<div class="flex flex-row justify-around items-center w-[40%] mx-[1rem] text-[3rem]">
		<div class="text-[6rem]">2048</div>
		<div class="flex w-[20rem] bg-[#edc22e]/90 text-black  p-[2rem] rounded-xl justify-start">
			<div class="">Score:</div>
			<span class=" ml-auto">{$gameScore}</span>
		</div>
	</div>
	<The2k48Grid bind:score={gameScore} />
</div>

<style lang="scss">
	@import url('https://fonts.cdnfonts.com/css/clear-sans');
	* {
		font-family: 'Clear Sans Medium', sans-serif;
		font-weight: 300;
	}
</style>
