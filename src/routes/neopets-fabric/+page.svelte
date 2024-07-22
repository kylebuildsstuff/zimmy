<script lang="ts">
	import { onMount } from 'svelte';
	import * as fabric from 'fabric';
	import grass from '$lib/assets/grass.jpg';

	let canvasEl: HTM;
	/**
	 * background doesn't set for some reason
	 *
	 */
	onMount(() => {
		const canvas = new fabric.Canvas(canvasEl);

		// Load background image
		// fabric.FabricImage.fromURL(grass, (img: any) => {
		// 	img.scaleToWidth(canvas.width);
		// 	img.scaleToHeight(canvas.height);
		// 	canvas.backgroundImage = img;
		// 	canvas.renderAll();
		// });

		fabric.Image.fromURL(grass, (img: any) => {
			img.scaleToWidth(canvas.width ?? 0);
			img.scaleToHeight(canvas.height ?? 0);
			canvas.backgroundImage = img;
			canvas.renderAll();
		});

		// Create shapes for shops
		const shop1 = new fabric.Circle({
			radius: 40,
			fill: 'red',
			left: 200,
			top: 300
		});

		const shop2 = new fabric.Rect({
			width: 80,
			height: 80,
			fill: 'blue',
			left: 500,
			top: 400
		});

		const shop3 = new fabric.Triangle({
			width: 70,
			height: 70,
			fill: 'purple',
			left: 800,
			top: 300
		});

		const shops = [shop1, shop2, shop3];

		shops.forEach((shop) => {
			shop.set({
				originX: 'center',
				originY: 'center',
				selectable: false,
				hoverCursor: 'pointer'
			});

			canvas.add(shop);
		});

		canvas.on('mouse:over', function (e) {
			if (e.target) {
				e.target.set('scaleX', 1.2);
				e.target.set('scaleY', 1.2);
				canvas.renderAll();
			}
		});

		canvas.on('mouse:out', function (e) {
			if (e.target) {
				e.target.set('scaleX', 1);
				e.target.set('scaleY', 1);
				canvas.renderAll();
			}
		});

		canvas.on('mouse:down', function (e) {
			if (e.target) {
				console.log(`Clicked on ${e.target.type}`);
			}
		});

		canvas.renderAll();
	});
</script>

<canvas bind:this={canvasEl} width="1024" height="768"></canvas>
