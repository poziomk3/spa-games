<script lang="ts">
	import WhackMoleGrid from './WhackMoleGrid.svelte';
	let fieldNumber = 2;
	let intervalDuration = 1000;
	let isRunning = false;
	let hits = 0;
	let misses = 0;
	let produced = 0;
	let passed = 0;
	let numberToRender = 5;

	$: if (hits + passed == numberToRender)
		setTimeout(() => {
			isRunning = false;
		}, 300);
	let onButton = () => {
		if (isRunning) {
			isRunning = false;
		} else {
			isRunning = true;
			hits = 0;
			produced = 0;
			passed = 0;
			misses = 0;
		}
	};
  $:innerWidth=0
</script>
<svelte:window bind:innerWidth/>
<div class="lg:grid grid-cols-12 py-4 h-screen text-[1.5rem] flex w-full justify-center">
	<div class="col-span-5 {isRunning&&innerWidth<1024?'hidden':''}   flex flex-col justify-center">
		<h1 class="text-[3rem] text-center">Whack a mole!</h1>

		<div class="flex justify-around">
			<div>Hits:<span class="text-[1.7em] ml-[1rem]">{hits}</span></div>
			<div>Misses:<span class="text-[1.7em] ml-[1rem]">{misses}</span></div>
			<div>Passed:<span class="text-[1.7em] ml-[1rem]">{passed}</span></div>
		</div>
		<div class="flex flex-col max-w-[15em] mx-auto pt-[1rem]">
			How many fields do you want?
			<div class="mx-auto flex w-full justify-around px-[0.5rem]">
				<div class="text-[1.5em]">{fieldNumber * fieldNumber}</div>
				<input class="ml-auto" type="range" bind:value={fieldNumber} min="2" max="10" />
			</div>

			How quick are you ?
			<div class="mx-auto flex w-full justify-around px-[0.5rem]">
				<div class="text-[1.5em]">{intervalDuration}ms</div>
				<input class="ml-auto" type="range" bind:value={intervalDuration} min="200" max="1500" />
			</div>

			How many moles?
			<div class="mx-auto flex w-full justify-around px-[0.5rem]">
				<div class="text-[1.5em]">{numberToRender}</div>
				<input class="ml-auto" type="range" bind:value={numberToRender} min="5" max="150" />
			</div>
		</div>

		<button class="mx-auto mt-[1rem] text-[1.5em] hover:scale-150 duration-300" on:click={onButton}
			>{isRunning ? 'Stop' : 'Start'}</button
		>
	</div>
	<div class="col-span-7 flex flex-col items-stretch justify-center w-full {!isRunning&&innerWidth<1024?'hidden':''} ">
		<WhackMoleGrid
			on:passed={() => {
				passed += 1;
			}}
			on:produced={() => {
				produced += 1;
			}}
			on:hit={() => {
				hits += 1;
			}}
			on:miss={() => {
				misses += 1;
			}}
			{produced}
			{numberToRender}
			{fieldNumber}
			{intervalDuration}
			{isRunning}
		/>
	</div>
</div>
