<script lang="ts">
	import FaHome from 'svelte-icons/fa/FaHome.svelte';

    let isOpen=false
    const handleClick=()=>{
        isOpen=!isOpen
        console.log("essa")
    }
</script>

<div>

	
    {#if isOpen}<div class="fixed h-screen w-[13rem] bg-red-200 top-0">
		<div class="flex flex-col">
			<a on:click={handleClick} href="/whack-a-mole">whack-a-mole</a>
			<a on:click={handleClick} href="/2k48">2048</a>
		</div>
	</div>
    {:else}
    <button class="w-[3rem] fixed top-[2rem] left-[2rem] hover:scale-125 z-10" on:click={handleClick}>
		<FaHome />
	</button>
    {/if}
	
</div>
