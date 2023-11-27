<script lang="ts">
	import FaHome  from 'svelte-icons/fa/FaHome.svelte';
	import { fly, scale } from 'svelte/transition';
	export const close=()=>{isOpen=false}
	let isOpen = true;
	const handleClick = () => {
		isOpen = !isOpen;
	};
</script>

<div>
	{#if isOpen}<div
			in:fly={{ duration: 300, x: -200 }}
			out:fly={{ duration: 300, x: -200 }}
			class="fixed h-screen w-[13rem] bg-neutral-100/50 backdrop-blur-sm top-0 text-black"
		>
			<div class="flex flex-col pt-[2rem] justify-center font-bold uppercase ">
				<h1 class='  text-center font-extrabold text-[2rem] '>Menu</h1>
				<a on:click={handleClick} class='pl-[1rem]' href="/whack-a-mole">whack-a-mole</a>
				<a on:click={handleClick} class='pl-[1rem]' href="/2k48">2048</a>
			</div>
		</div>
	{:else}
		<button
			class="w-[3rem] fixed top-[2rem] left-[2rem] hover:scale-125"
			in:scale
			out:scale
			on:click={handleClick}
		>
			<FaHome />
		</button>
	{/if}
</div>
