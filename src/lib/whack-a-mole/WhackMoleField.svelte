<script lang="ts">
  import { createEventDispatcher, onMount } from "svelte";
  export let isMole = false;
  $: isThereTarget = false;
  $: isShot = false;
  let firstRender = true;
  let duration = 50;

  $: if (!isMole && !firstRender) {
    isThereTarget = true;
    setTimeout(() => {
      isThereTarget = false;
    }, duration);
  }
  onMount(() => {
    setTimeout(() => {
      firstRender = false;
    }, duration+100);
  });
  function showHitHandle() {
    console.log("hit");
    isShot = true;
    setTimeout(() => {
      isShot = false;
    }, duration);
  }
  const dispatch = createEventDispatcher();
</script>



<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
  class="{isThereTarget && isShot
    ? 'bg-green-700'
    : !isThereTarget && isShot
      ? 'bg-red-700'
      : isThereTarget
        ? 'bg-black'
        : ''}  bg-opacity-60 flex flex-col justify-center items-center border-4 rounded-xl w-full h-full active:scale-[.025]"
  on:mousedown={() => {
    dispatch("whack", { isMole });
    showHitHandle();
  }}
>
  {#if isMole}
    <div class="bg-red-500 w-10 h-10 rounded-full"></div>
  {:else}
    <div class="bg-blue-500 w-10 h-10 rounded-full"></div>
  {/if}
</div>
