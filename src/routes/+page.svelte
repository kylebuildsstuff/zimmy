<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { browser } from '$app/environment';
	import {
		Frame,
		Dial,
		Emitter,
		Pen,
		MotionController,
		series,
		LabelOnArc,
		ProgressBar,
		Blob,
		Stepper,
		Button,
		Stage,
		Rectangle,
		Slider,
		Poly,
		Parallax,
		Accelerator,
		Scroller,
		Dynamo,
		Tile
		// Style
	} from 'zimjs';
	import { slide } from 'svelte/transition';
	// import { createjs } from 'createjs';

	// https://readmedium.com/your-guide-to-components-on-the-javascript-canvas-54e0d8fc79b
	new Frame({
		scaling: FIT,
		width: 1024,
		height: 768,
		color: light,
		outerColor: dark,
		progress: new ProgressBar(),
		ready: (frame, stage, width, height, mobile) => {
			/**
			 * Random shapes
			 */
			// new Blob().center();
			// new Emitter().center();

			console.log('frame: ', frame);
			console.log('stage: ', stage);
			console.log('width: ', width);
			console.log('height: ', height);
			console.log('mobile: ', mobile);

			/**
			 * Button interactivity
			 */
			// const button = new Button(250, 80, 'EXPLODE', red, grey).center();
			// button.on('click', () => {
			// 	button.removeFrom();
			// 	new Emitter(new Rectangle(50, 50, [grey, red])).center();
			// 	// frame.update()
			// });

			/**
			 * Circle and slider scaling
			 */
			// const circle = new Circle(20, red).center().sca(5);
			// const slider = new Slider().center();

			// slider.on('change', () => {
			// 	circle.scale = slider.currentValue;
			// 	stage.update();
			// });

			// https://readmedium.com/controlling-the-canvas-with-javascript-d94792aa33cd
			/**
			 * Motions (car)
			 */
			// const car = new Rectangle(120, 60).centerReg();
			// new Rectangle(50, 50, yellow).center(car);
			// new MotionController({
			// 	target: car,
			// 	type: 'mousemove',
			// 	rotate: true
			// });

			/**
			 * Motions (emitter)
			 */
			// new Emitter(new Poly(200, 6, 0.6, yellow, dark)).center();
			// new Emitter({
			// 	obj: new Poly({
			// 		radius: { min: 50, max: 100 },
			// 		sides: [5, 6, 7],
			// 		pointSize: { min: 0.5, max: 0.8 },
			// 		color: [yellow, orange, red],
			// 		borderColor: dark
			// 	}),
			// 	interval: 0.8,
			// 	num: 30,
			// 	force: { min: 4, max: 8 },
			// 	life: 2,
			// 	angle: { min: -90 - 20, max: -90 + 20 },
			// 	animation: {
			// 		props: { rotation: [360, -360] },
			// 		time: { min: 1, max: 3 },
			// 		loop: true
			// 	}
			// }).center();

			/**
			 * Pen
			 */
			// new MotionController({
			// 	target: new Pen().addTo(),
			// 	type: 'pressmove'
			// });

			/**
			 * Pen 2
			 */
			// const rect = new Rectangle(stage.width, stage.height).addTo();
			// new MotionController({
			// 	target: new Pen({
			// 		penType: 'kitetail',
			// 		size: { min: 10, max: 80 }
			// 	}).addTo(),
			// 	type: 'pressmove',
			// 	speed: 100,
			// 	damp: 1,
			// 	mousedownIncludes: rect
			// });

			/**
			 * Parallax
			 */
			// const back = new Rectangle(800, 200, yellow).center();
			// const mid = new Rectangle(400, 200, green).center().mov(0, 30);
			// const front = new Circle(80, red).center().mov(0, 60);

			// const parallax = new Parallax([
			// 	{ obj: back, prop: 'x', propChange: 50 },
			// 	{ obj: back, prop: 'y', propChange: 40, input: 'mouseY' },
			// 	{ obj: mid, prop: 'x', propChange: 125 },
			// 	{ obj: mid, prop: 'y', propChange: 80, input: 'mouseY' },
			// 	{ obj: front, prop: 'x', propChange: 200 },
			// 	{ obj: front, prop: 'y', propChange: 100, input: 'mouseY' }
			// ]);

			/**
			 * Accelerator (doesn't work)
			 */
			// const accelerator = new Accelerator([
			// 	new Scroller({ backing: 'background', speed: 1.5 }),
			// 	new Dynamo({ sprite: 'spaceGuy', reversible: false })
			// ]);

			// new MotionController({
			// 	target: accelerator,
			// 	type: 'mousemove',
			// 	axis: 'horizontal',
			// 	speed: 100,
			// 	minPercentSpeed: -250,
			// 	maxPercentSpeed: 250
			// });

			/**
			 * Animate https://readmedium.com/your-guide-to-animation-on-the-canvas-with-javascript-cb928c4c888a
			 */
			// with CreateJS
			// const circle = new Circle().center();
			// createjs.Tween.get(circle).to({ x: 1000 }, 1000).call(removeTicker);

			// const ticker = createjs.Ticker.on('tick', stage);
			// function removeTicker() {
			// 	createjs.Ticker.off('tick', ticker);
			// }

			// With just zim (which uses createJS under the hood)
			// new Circle().center().animate({ x: 1000 });

			// Rewind and loop
			// new Circle().center().animate({
			// 	props: {
			// 		scale: 3,
			// 		color: red
			// 	},
			// 	time: 2,
			// 	rewind: true,
			// 	loop: true
			// });

			// Sequence
			// const tile = new Tile()
			// 	.alp(0.5)
			// 	.centerReg()
			// 	.animate({
			// 		props: { scale: 1.5 },
			// 		time: 0.4,
			// 		rewind: true,
			// 		sequence: 0.2,
			// 		call: () => {
			// 			tile.animate({ rotation: 360 });
			// 		}
			// 	});

			/**
			 * Style
			 * https://readmedium.com/style-on-the-canvas-12660313ba6a
			 */

			// new Button({
			// 	width: 500,
			// 	corner: 100
			// }).center();

			// new Slider({ group: 'big' }).pos(0, 100, CENTER);
			// new Dial({ group: 'big' }).center();

			/**
			 * Board
			 *
			 * https://readmedium.com/en/https:/levelup.gitconnected.com/make-html-5-games-isometric-board-game-tutorial-439783c1d35d
			 */
			//  const board = new Board().center()
		}
	});
</script>

<div id="zim" />
