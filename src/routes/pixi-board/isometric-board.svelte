<script lang="ts">
	import { onMount } from 'svelte';
	import * as PIXI from 'pixi.js';

	let canvas: HTMLCanvasElement;
	let app: PIXI.Application;

	onMount(() => {
		app = new PIXI.Application({
			view: canvas,
			width: 800,
			height: 600,
			backgroundColor: 0xaaaaaa
		});

		// Wait for the next frame to ensure the application is fully initialized
		requestAnimationFrame(() => {
			createIsometricBoard();
		});

		return () => {
			if (app) {
				app.destroy(true, { children: true, texture: true, baseTexture: true });
			}
		};
	});

	function createIsometricBoard() {
		if (!app) return;

		// Create a container for the isometric board
		const board = new PIXI.Container();
		app.stage.addChild(board);

		// Define tile dimensions and board size
		const tileWidth = 60;
		const tileHeight = 30;
		const boardSize = 16;

		// Create isometric tiles
		for (let row = 0; row < boardSize; row++) {
			for (let col = 0; col < boardSize; col++) {
				const tile = new PIXI.Graphics();

				// Draw diamond shape
				tile.beginFill((row + col) % 2 === 0 ? 0xe0e0e0 : 0xb0b0b0);
				tile.moveTo(0, -tileHeight / 2);
				tile.lineTo(tileWidth / 2, 0);
				tile.lineTo(0, tileHeight / 2);
				tile.lineTo(-tileWidth / 2, 0);
				tile.lineTo(0, -tileHeight / 2);
				tile.endFill();

				// Position tiles
				tile.x = ((col - row) * tileWidth) / 2;
				tile.y = ((col + row) * tileHeight) / 2;

				board.addChild(tile);

				// Add hover effect
				tile.eventMode = 'static';
				tile.on('mouseover', () => {
					tile.scale.set(1.1);
				});
				tile.on('mouseout', () => {
					tile.scale.set(1.0);
				});
			}
		}

		// Center the board
		board.x = app.screen.width / 2;
		board.y = app.screen.height / 2;

		// Variables for dragging
		let isDragging = false;
		let dragStart = new PIXI.Point();

		// Make the board draggable
		board.eventMode = 'static';
		board
			.on('pointerdown', onDragStart)
			.on('pointerup', onDragEnd)
			.on('pointerupoutside', onDragEnd)
			.on('pointermove', onDragMove);

		function onDragStart(event: PIXI.FederatedPointerEvent) {
			isDragging = true;
			dragStart.copyFrom(event.global);
			board.alpha = 0.8;
		}

		function onDragEnd() {}

		function onDragMove(event: PIXI.FederatedPointerEvent) {
			if (isDragging) {
				const newPosition = event.global;
				board.x += newPosition.x - dragStart.x;
				board.y += newPosition.y - dragStart.y;
				dragStart.copyFrom(newPosition);
			}
		}
	}
</script>
