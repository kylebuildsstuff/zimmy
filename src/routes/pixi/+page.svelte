<script>
	import { onMount } from 'svelte';
	import * as PIXI from 'pixi.js';

	let app;
	let pixiContainer;

	onMount(async () => {
		app = new PIXI.Application();
		await app.init({ width: 640, height: 360, background: '#1099bb' });

		// Create a new Text object
		const text = new PIXI.Text('Hello World', {
			fontFamily: 'Arial',
			fontSize: 36,
			fill: 0xffffff,
			align: 'center'
		});

		// Center the text
		text.x = app.screen.width / 2;
		text.y = app.screen.height / 2;
		text.anchor.set(0.5);

		// Add the text to the stage
		app.stage.addChild(text);

		// Add the PixiJS view to the DOM
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

<div bind:this={pixiContainer}></div>

<style>
	div {
		width: 640px;
		height: 360px;
	}
</style>
