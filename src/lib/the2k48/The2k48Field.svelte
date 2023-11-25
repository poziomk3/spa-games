<script lang="ts">
	export let position: [number, number];
	export let value: number|undefined;
	export let fieldsNumber: number;
</script>

<div
	style="--position-x:{position[0]};--position-y:{position[1]};--percentage:{100 / fieldsNumber}"
	class=" cursor-pointer position absolute bg-blue-500 scale-90 rounded-xl transition-all duration-100"
>
<div class=" flex flex-col justify-center items-center relative h-full cursor-pointer"><span class=" cursor-pointer  text-[2rem] text-bold ">{value}</span></div></div>

<style lang="scss">
	.position {
		height: calc(var(--percentage) * 1%);
		width: calc(var(--percentage) * 1%);
		left: calc(var(--position-x) * calc(var(--percentage) * 1%));
		top: calc(var(--position-y) * calc(var(--percentage) * 1%));
	}
</style>
