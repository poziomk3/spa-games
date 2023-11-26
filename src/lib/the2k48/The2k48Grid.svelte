<script lang="ts">
	import { onMount, tick } from 'svelte';
	import The2k48Field from '$lib/the2k48/The2k48Field.svelte';
	import { writable } from 'svelte/store';


	export const score = writable(0);
	$: {
		const newScore = Array.from(gameMap.values()).reduce((acc, curr) => acc + curr, 0);
		score.set(newScore);
	}

	type Position = [number, number];
	type Arrow = 'ArrowUp' | 'ArrowDown' | 'ArrowLeft' | 'ArrowRight';

	const fieldsNumber = 4;
	let fields: [number, number][] = Array.from({ length: fieldsNumber }, (_, i) =>
		Array.from({ length: fieldsNumber }, (_, j): Position => [i, j])
	).flat();
	let gameMap: Map<Position, number> = new Map(fields.map((field) => [field, 0]));
	let tiles: Map<string, Position> = new Map();

	function spawn() {
		let emptyFields = fields.filter((field) => gameMap.get(field) === 0);
		if (emptyFields.length === 0) return;
		let randomField = emptyFields[Math.floor(Math.random() * emptyFields.length)];
		gameMap.set(randomField, Math.random() < 0.9 ? 2 : 4);
		gameMap = new Map(gameMap.entries());
		tiles.set(Date.now().toString(), randomField);
		tiles = new Map(tiles.entries());
	}
	function getKey(newPos: Position): Position {
		for (let key of gameMap.keys()) {
			if (key[0] == newPos[0] && key[1] == newPos[1]) return key;
		}
		return newPos;
	}

	function isWithinBounds(position: Position): boolean {
		return (
			position[0] >= 0 &&
			position[0] < fieldsNumber &&
			position[1] >= 0 &&
			position[1] < fieldsNumber
		);
	}
	function move(
		tile: [string, Position],
		arg: Arrow,
		transformed: Array<string>
	): [boolean, Array<string>] {
		let shifter: (arg: Position) => Position;
		switch (arg) {
			case 'ArrowLeft':
				shifter = ([x, y]) => [x - 1, y];
				break;
			case 'ArrowRight':
				shifter = ([x, y]) => [x + 1, y];
				break;
			case 'ArrowUp':
				shifter = ([x, y]) => [x, y - 1];
				break;
			case 'ArrowDown':
				shifter = ([x, y]) => [x, y + 1];
				break;
		}
		let [id, position] = tile;
		const privGameMap = new Map(gameMap.entries());
		const privTiles = new Map(tiles.entries());

		function processTile(position: Position, newPos: Position): string | boolean {
			if (privGameMap.get(newPos) === 0) {
				privTiles.set(id, newPos);
				privGameMap.set(newPos, privGameMap.get(position)!);
				privGameMap.set(position, 0);
				for (const [key, positionPriv] of privTiles.entries()) {
					if (privGameMap.get(getKey(positionPriv)) == 0) privTiles.delete(key);
				}
				return true;
			} else if (privGameMap.get(newPos) === privGameMap.get(position)) {
				for (const [key, positionPriv] of privTiles.entries()) {
					if (positionPriv == newPos) {
						if (transformed.includes(key)) {
							return false;
						}
						privTiles.delete(key);
						break;
					}
				}

				privGameMap.set(newPos, privGameMap.get(newPos)! * 2);
				privGameMap.set(position, 0);
				privTiles.set(id, newPos);
				for (const [key, positionPriv] of privTiles.entries()) {
					if (privGameMap.get(getKey(positionPriv)) == 0) privTiles.delete(key);
				}
				return id;
			}

			return false;
		}

		position = getKey(position);
		let newPos: Position;
		let changeFlag = false;

		while (isWithinBounds(shifter(position))) {
			newPos = getKey(shifter(position));
			const result = processTile(position, newPos);
			if (result === true) {
				changeFlag = true;
				position = newPos;
			} else if (result === id) {
				transformed.push(id);
				changeFlag = true;
				break;
			} else break;
		}
		gameMap = new Map(privGameMap.entries());
		tiles = new Map(privTiles.entries());
		return [changeFlag, [...transformed]];
	}

	function iterator(type: Arrow): IterableIterator<[string, Position]> {
		let predicate: (value1: Position, value2: Position) => number;
		switch (type) {
			case 'ArrowUp':
				predicate = (value1, value2) => value1[1] - value2[1];
				break;
			case 'ArrowDown':
				predicate = (value1, value2) => value2[1] - value1[1];
				break;
			case 'ArrowLeft':
				predicate = (value1, value2) => value1[0] - value2[0];
				break;
			case 'ArrowRight':
				predicate = (value1, value2) => value2[0] - value1[0];
				break;
		}
		return new Map(
			[...tiles].sort(([key1, value1], [key2, value2]) => predicate(value1, value2))
		).entries();
	}

	function iterateTiles(type: Arrow): boolean {
		let transformed: Array<string> = new Array();
		let changeFlag = false;
		for (let [id, position] of iterator(type)) {
			const [flag, newTransformed] = move([id, position], type, transformed);
			if (flag) {
				changeFlag = true;
				transformed = newTransformed;
			}
		}
		return changeFlag;
	}

	function onKeyDown(e: KeyboardEvent): void {
		const { key } = e;
		let changeFlag = false;
		switch (key) {
			case 'ArrowUp':
				changeFlag = iterateTiles('ArrowUp');
				break;
			case 'ArrowDown':
				changeFlag = iterateTiles('ArrowDown');
				break;
			case 'ArrowLeft':
				changeFlag = iterateTiles('ArrowLeft');
				break;
			case 'ArrowRight':
				changeFlag = iterateTiles('ArrowRight');
				break;
		}

		if (changeFlag) spawn();
	}

	onMount(async () => {
		spawn();
		await tick();
		spawn();
	});

	
</script>

<svelte:window on:keydown={onKeyDown} />
<div class="relative  bg-neutral-600/50  aspect-square md:flex-grow     m-[1rem]">
	{#each gameMap as [position, value]}
		<The2k48Field {position} value={0} {fieldsNumber} />
	{/each}
	{#each tiles as [id, position] (id)}
		<The2k48Field {position} value={gameMap.get(position)} {fieldsNumber} />
	{/each}
</div>
