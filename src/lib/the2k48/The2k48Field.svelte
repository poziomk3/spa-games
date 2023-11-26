<script lang="ts">
	import { scale } from "svelte/transition";

	export let position: [number, number];
	export let value: number | undefined;
	export let fieldsNumber: number;
</script>

<div
	in:scale
	out:scale
	style="--position-x:{position[0]};--position-y:{position[1]};--percentage:{100 / fieldsNumber}"
	class=" text-[10vmin] cursor-pointer position absolute scale-90 rounded-xl transition-all duration-100 bg-white/70 n{value?.toString()}"
>
	<div class=" flex flex-col justify-center items-center relative h-full cursor-pointer ">
		{#if value != 0}
			<span class=" cursor-pointer text-bold">{value}</span>
		{/if}
	</div>
</div>

<style lang="scss">
	@import url('https://fonts.cdnfonts.com/css/clear-sans');
	* {
		font-family: 'Clear Sans Medium', sans-serif;
		font-weight: 900;
	}
	.position {
		height: calc(var(--percentage) * 1%);
		width: calc(var(--percentage) * 1%);
		left: calc(var(--position-x) * calc(var(--percentage) * 1%));
		top: calc(var(--position-y) * calc(var(--percentage) * 1%));
	}

	.n2 {
		background-color: #eee4da;
		color: #776e65;
	}
	.n4 {
		background-color: #ede0c8;
		color: #776e65;
	}
	.n8 {
		background-color: #f2b179;
		color: #f9f6f2;
	}
	.n16 {
		background-color: #f59563;
		color: #f9f6f2;
	}
	.n32 {
		background-color: #f67c5f;
		color: #f9f6f2;
	}
	.n64 {
		background-color: #f65e3b;
		color: #f9f6f2;
	}
	.n128 {
		background-color: #edcf72;
		color: #f9f6f2;
	}
	.n256 {
		background-color: #edcc61;
		color: #f9f6f2;
	}
	.n512 {
		background-color: #edc850;
		color: #f9f6f2;
	}
	.n1024 {
		background-color: #edc53f;
		color: #f9f6f2;
	}
	.n2048 {
		background-color: #edc22e;
		color: #f9f6f2;
	}
	.n4096 {
		background-color: #3c3a32;
		color: #f9f6f2;
	}
</style>
