<script lang="ts">
	import { Frame, Pic, Container, Shape, Rectangle } from 'zimjs';
	import { Board } from '@zimjs/game';
	import block from '$lib/assets/blocks_1-45.png';

	/**
	 * Written by Claude 3.5 (doesn't work)
	 */
	// Initialize ZIM Frame
	var frame = new Frame({
		scaling: FIT,
		width: 1024,
		height: 768,
		color: light,
		outerColor: dark,
		ready: () => {
			const stage = frame.stage;
			const stageW = frame.width;
			const stageH = frame.height;

			// Create a container for our isometric board
			var board = new Container(stageW, stageH).addTo(stage);
			board.center(stage);

			// Define tile dimensions
			const tileWidth = 50;
			const tileHeight = 25;

			// Create isometric tiles
			for (let row = 0; row < 8; row++) {
				for (let col = 0; col < 8; col++) {
					let tile = new Rectangle(tileWidth, tileHeight).addTo(board);

					// Rotate and position tiles
					tile.rotation = 45;
					tile.x = ((col - row) * tileWidth) / 2;
					tile.y = ((col + row) * tileHeight) / 2;

					// Alternate colors for checkerboard effect
					tile.fill = (row + col) % 2 == 0 ? '#e0e0e0' : '#b0b0b0';
				}
			}

			// Add interactivity
			board.on('mouseover', function (e) {
				if (e.target instanceof Rectangle) {
					e.target.animate({ scale: 1.1 }, { duration: 300 });
				}
			});

			board.on('mouseout', function (e) {
				if (e.target instanceof Rectangle) {
					e.target.animate({ scale: 1 }, { duration: 300 });
				}
			});

			stage.update();
		}
	});
</script>
