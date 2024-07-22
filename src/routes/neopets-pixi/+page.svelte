<script>
	import { onMount } from 'svelte';
	import * as PIXI from 'pixi.js';
	import { gsap } from 'gsap';
	import grass from '$lib/assets/grass.jpg';

	let pixiContainer;

	onMount(async () => {
		const app = new PIXI.Application({
			width: 1024,
			height: 768,
			backgroundColor: 0x1099bb
		});

		// Wait for the application to be ready
		await app.init();

		// Now we can safely append the view
		pixiContainer.appendChild(app.view);

		const texture = await PIXI.Assets.load(grass);

		// Create background
		const background = PIXI.Sprite.from(texture);
		background.width = app.screen.width;
		background.height = app.screen.height;
		app.stage.addChild(background);

		// Create shops
		const shop1 = new PIXI.Graphics();
		shop1.beginFill(0xff0000);
		shop1.drawCircle(0, 0, 40);
		shop1.endFill();
		shop1.position.set(200, 300);

		const shop2 = new PIXI.Graphics();
		shop2.beginFill(0x0000ff);
		shop2.drawRect(-40, -40, 80, 80);
		shop2.endFill();
		shop2.position.set(500, 400);

		const shop3 = new PIXI.Graphics();
		shop3.beginFill(0x800080);
		shop3.moveTo(0, -35);
		shop3.lineTo(35, 35);
		shop3.lineTo(-35, 35);
		shop3.closePath();
		shop3.endFill();
		shop3.position.set(800, 300);

		const shops = [shop1, shop2, shop3];

		shops.forEach((shop, index) => {
			shop.eventMode = 'static';
			shop.cursor = 'pointer';

			shop.on('pointerover', () => {
				gsap.to(shop.scale, {
					x: 1.2,
					y: 1.2,
					duration: 0.3,
					ease: 'power2.out'
				});
			});

			shop.on('pointerout', () => {
				gsap.to(shop.scale, {
					x: 1,
					y: 1,
					duration: 0.3,
					ease: 'power2.out'
				});
			});

			shop.on('pointerdown', () => {
				console.log(`Clicked on shop ${index + 1}`);
			});

			app.stage.addChild(shop);
		});

		return () => {
			app.destroy(true, { children: true, texture: true, baseTexture: true });
		};
	});
</script>

<div bind:this={pixiContainer}></div>
