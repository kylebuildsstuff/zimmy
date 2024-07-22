<script lang="ts">
	import { Bitmap, Circle, Frame, Pic, Rectangle, Triangle } from 'zimjs';
	import { Board } from '@zimjs/game';
	import grass from '$lib/assets/grass.jpg';

	new Frame({
		scaling: FIT,
		width: 1024,
		height: 768,
		color: light,
		outerColor: dark,
		ready: (frame, stage, width, height, mobile) => {
			const what = new Pic(grass).center();

			// const bitmap = new Bitmap(what, 1024, 768, 0, 0).center();

			// Create three different shapes for shops
			const shop1 = new Circle(40, 'red').pos(200, 300);
			const shop2 = new Rectangle(80, 80, 'blue').pos(500, 400);
			const shop3 = new Triangle(70, 70, 70, 'purple').pos(800, 300);

			const shops = [shop1, shop2, shop3];

			shops.forEach((shop) => {
				shop.cursor = 'pointer';

				// Grow on hover
				shop.on('mouseover', () => {
					shop.animate({
						target: shop,
						scale: 1.2,
						time: 0.3
					});
				});

				// Shrink back on mouseout
				shop.on('mouseout', () => {
					shop.animate({
						target: shop,
						scale: 1,
						time: 0.3
					});
				});

				// Optional: Add click event
				shop.on('click', () => {
					console.log(`Clicked on ${shop.constructor.name}`);
				});

				stage.addChild(shop);
			});
		}
	});
</script>
