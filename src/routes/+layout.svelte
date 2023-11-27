<script lang="ts">
	import '../app.css';
	import Navigation from '$lib/common/Navigation.svelte';
	let close: (() => void) | undefined;
</script>

<!-- <svelte:window on:click={close} /> -->
<div class="h-screen bg-image">
	<Navigation bind:close />

	<button on:click={close} class="w-full h-full">
		<slot />
	</button>
</div>

<style lang="scss">
	@import url('https://fonts.googleapis.com/css2?family=Anybody:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Lumanosimo&family=Permanent+Marker&family=Raleway:wght@400;500;700&display=swap');
	:global(body) {
		font-family: 'Permanent Marker', cursive;
		color: white;
	}
	.bg-image {
		background-image: url('images/bg.jpg');
		backdrop-filter: blur(10%);
	}
</style>
