<script lang="ts">
	import { afterUpdate, onMount } from 'svelte';
	import The2k48Field from '$lib/the2k48/The2k48Field.svelte';

	const fieldsNumber = 5;
	let fields: [number, number][] = createFields(fieldsNumber);

	let gameMap: Map<[number, number], number> = new Map(fields.map((field) => [field, 0]));
	let tiles: Map<string,[number,number]> = new Map();

	function createFields(num: number): [number, number][] {
		let fields = Array<[number, number]>();
		for (let i = 0; i < num; i++) {
			for (let j = 0; j < num; j++) {
				fields.push([i, j]);
			}
		}
		return fields;
	}

	function spawn() {
		let emptyFields = fields.filter((field) => gameMap.get(field) === 0);
		if (emptyFields.length === 0) return;
		let randomField = emptyFields[Math.floor(Math.random() * emptyFields.length)];
		gameMap.set(randomField, 2);
		gameMap = new Map(gameMap.entries());
		tiles.set(Date.now().toString(),randomField)
		tiles = new Map(tiles.entries());
	}


    function move(tile:[string,[number,number]],arg:'left'|'right'|'up'|'down'){
        let shifter:(arg:[number,number])=>[number,number]

        switch(arg){
            case 'left':
                shifter=([x,y])=>[x-1,y]
                break;
            case 'right':
                shifter=([x,y])=>[x+1,y]
                break;
            case 'up':
                shifter=([x,y])=>[x,y-1]
                break;
            case 'down':
                shifter=([x,y])=>[x,y+1]
                break;
        }
        let  [id,position]=tile

        const privGameMap=new Map(gameMap.entries());
        const privTiles=new Map(tiles.entries());

        let newPos:[number,number];
                while(shifter(position)[0]>=0&&shifter(position)[0]<fieldsNumber&&shifter(position)[1]>=0&&shifter(position)[1]<fieldsNumber){

                newPos=shifter(position)
                privGameMap.set(position,privGameMap.get(position)!)
                privTiles.set(id,newPos)
                
                privGameMap.set(newPos,privGameMap.get(position)!)
                privGameMap.set(position,0)
                console.log(privGameMap.get(position))
                position = newPos
            }


        gameMap = new Map(privGameMap.entries());
        console.log('w funckji',gameMap)
        tiles = new Map(privTiles.entries());
    }
	function onKeyDown(e: any) {
		const { keyCode } = e;
		switch (keyCode) {
			case 38:
            tiles.forEach((tile,value)=>{
                    move([value,tile],'up')
                })
				break;
			case 40:
            tiles.forEach((tile,value)=>{
                    move([value,tile],'down')
                })
				break;
			case 37:
            tiles.forEach((tile,value)=>{
                    move([value,tile],'left')
                })
				break;
			case 39:
            tiles.forEach((tile,value)=>{
                    move([value,tile],'right')
                })
				break;
		}
		// spawn();
      
	}
 
	onMount(() => {
		spawn();
	});
</script>

<svelte:window on:keydown={onKeyDown} />

<div class="relative w-[400px] h-[400px] bg-red-200">
	{#each tiles as [id, position] (id)}
		{#if gameMap.get(position) != 0}
			<The2k48Field {position} value={gameMap.get(position)} {fieldsNumber} />
		{/if}
	{/each}
</div>
{#each tiles as [id, position] (id)}
	<div>{id} essa {position}</div>
    <div></div>
{/each}
nowe
{#each gameMap as [position,value] }
{#if value!=0}
<div>{value} {position}</div>
{/if}
	
{/each}