<script lang="ts">
	import { onMount } from 'svelte';
	import * as PIXI from 'pixi.js';

	let app;
	let pixiContainer;

	const TILE_SIZE = 32;
	const MAP_WIDTH = 20;
	const MAP_HEIGHT = 15;

	// Simple map representation (0 for grass, 1 for trees)
	const map = Array(MAP_HEIGHT)
		.fill()
		.map(() => Array(MAP_WIDTH).fill(0));
	for (let i = 0; i < 50; i++) {
		const x = Math.floor(Math.random() * MAP_WIDTH);
		const y = Math.floor(Math.random() * MAP_HEIGHT);
		map[y][x] = 1;
	}

	let player;

	onMount(async () => {
		app = new PIXI.Application();
		await app.init({
			width: MAP_WIDTH * TILE_SIZE,
			height: MAP_HEIGHT * TILE_SIZE + 40,
			background: '#87CEFA'
		});

		// Create tile sprites
		const grassTexture = PIXI.Texture.from('https://pixijs.com/assets/bunny.png'); // Replace with actual grass texture
		const treeTexture = PIXI.Texture.from('https://pixijs.com/assets/bunny.png'); // Replace with actual tree texture

		const mapContainer = new PIXI.Container();
		for (let y = 0; y < MAP_HEIGHT; y++) {
			for (let x = 0; x < MAP_WIDTH; x++) {
				const tile = new PIXI.Sprite(map[y][x] === 0 ? grassTexture : treeTexture);
				tile.width = tile.height = TILE_SIZE;
				tile.x = x * TILE_SIZE;
				tile.y = y * TILE_SIZE;
				mapContainer.addChild(tile);
			}
		}
		app.stage.addChild(mapContainer);

		// Create player
		player = new PIXI.Graphics();
		player.beginFill(0xff0000);
		player.drawCircle(0, 0, TILE_SIZE / 2);
		player.endFill();
		player.x = TILE_SIZE / 2;
		player.y = TILE_SIZE / 2;
		mapContainer.addChild(player);

		// Create mini-map
		const miniMap = new PIXI.Graphics();
		miniMap.beginFill(0xcccccc);
		miniMap.drawRect(0, 0, MAP_WIDTH * 2, MAP_HEIGHT * 2);
		miniMap.endFill();
		miniMap.x = app.screen.width - MAP_WIDTH * 2 - 10;
		miniMap.y = 10;
		app.stage.addChild(miniMap);

		for (let y = 0; y < MAP_HEIGHT; y++) {
			for (let x = 0; x < MAP_WIDTH; x++) {
				const dot = new PIXI.Graphics();
				dot.beginFill(map[y][x] === 0 ? 0x00ff00 : 0x008000);
				dot.drawRect(x * 2, y * 2, 2, 2);
				dot.endFill();
				miniMap.addChild(dot);
			}
		}

		// Create status bar
		const statusBar = new PIXI.Graphics();
		statusBar.beginFill(0x333333);
		statusBar.drawRect(0, app.screen.height - 40, app.screen.width, 40);
		statusBar.endFill();
		app.stage.addChild(statusBar);

		const statusText = new PIXI.Text('Neotopia - X: 0, Y: 0', {
			fontFamily: 'Arial',
			fontSize: 16,
			fill: 0xffffff
		});
		statusText.x = 10;
		statusText.y = app.screen.height - 30;
		app.stage.addChild(statusText);

		// Handle keyboard input
		window.addEventListener('keydown', (e) => {
			let newX = player.x;
			let newY = player.y;
			switch (e.key) {
				case 'ArrowUp':
					newY -= TILE_SIZE;
					break;
				case 'ArrowDown':
					newY += TILE_SIZE;
					break;
				case 'ArrowLeft':
					newX -= TILE_SIZE;
					break;
				case 'ArrowRight':
					newX += TILE_SIZE;
					break;
			}
			if (newX >= 0 && newX < app.screen.width && newY >= 0 && newY < app.screen.height - 40) {
				const mapX = Math.floor(newX / TILE_SIZE);
				const mapY = Math.floor(newY / TILE_SIZE);
				if (map[mapY][mapX] === 0) {
					player.x = newX;
					player.y = newY;
					statusText.text = `Neotopia - X: ${mapX}, Y: ${mapY}`;
				}
			}
		});

		pixiContainer.appendChild(app.view);
	});

	onMount(() => {
		return () => {
			if (app) {
				app.destroy(true, { children: true, texture: true, baseTexture: true });
			}
		};
	});
</script>

<div bind:this={pixiContainer} />
