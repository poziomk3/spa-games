<script>
  import WhackMoleGrid from "./WhackMoleGrid.svelte";
  let fieldNumber = 2;
  let intervalDuration = 1000;
  let isRunning = false;
  let hits = 0;
  let misses = 0;
  let produced=0;
  let passed=0;
  let numberToRender=5;

  $ :if(hits+passed==numberToRender) setTimeout(()=>{isRunning = false},300);
  let onButton=()=>{
    if  (isRunning){
      isRunning = false;
    }
    else{
    isRunning = true;
    hits=0;
    produced=0;
    passed=0;
    misses=0;}
  }
</script>

<div class="grid  grid-cols-12  py-4 h-screen">
  <div class="col-span-2 flex flex-col justify-center items-center">
    <h1>Whack a mole!</h1>
  
  
   
      <button
        on:click={onButton}>{isRunning ? "Stop" : "Start"}</button
      >
      <br />
      <div>Hits:<span>{hits}</span></div>
      <div>Misses:<span>{misses}</span></div>
      <div>Produced:<span>{produced}</span></div>
      <div>Passed:<span>{passed}</span></div>

      How many fields do you want?
      {fieldNumber * fieldNumber}
      <input type="range" bind:value={fieldNumber} min="2" max="10" />
      How quick are you [ms]?
      {intervalDuration}
      <input type="range" bind:value={intervalDuration} min="200" max="1500" />
      How many moles?
      {numberToRender }
      <input type="range" bind:value={numberToRender} min="5" max="150" />
  </div>
  
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
