<script lang="ts">
	import { Frame, Pic, Container, Shape, Rectangle } from 'zimjs';

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
			var board = new Container().addTo(stage);
			board.center(stage);

			// Define tile dimensions and board size
			const tileWidth = 60;
			const tileHeight = 30;
			const boardSize = 16;

			// Create isometric tiles
			for (let row = 0; row < boardSize; row++) {
				for (let col = 0; col < boardSize; col++) {
					let tile = new Shape().addTo(board);

					tile.graphics
						.beginFill((row + col) % 2 == 0 ? '#e0e0e0' : '#b0b0b0')
						.moveTo(0, -tileHeight / 2)
						.lineTo(tileWidth / 2, 0)
						.lineTo(0, tileHeight / 2)
						.lineTo(-tileWidth / 2, 0)
						.lineTo(0, -tileHeight / 2)
						.endFill();

					tile.x = ((col - row) * tileWidth) / 2;
					tile.y = ((col + row) * tileHeight) / 2;

					tile.on('mouseover', () => (tile.scale = 1.1));
					tile.on('mouseout', () => (tile.scale = 1));
				}
			}

			// Make the board draggable using ZIM's built-in functionality
			board.drag({
				boundary: new Rectangle(
					-board.getBounds()?.width + stageW,
					-board.getBounds()?.height + stageH,
					board.getBounds()?.width,
					board.getBounds()?.height
				)
			});

			// Add a background to clearly see the draggable area
			var background = new Rectangle(stageW, stageH, 'rgba(200,200,200,.2)').addTo(stage);
			background.pos(0, 0);
			stage.setChildIndex(background, 0);

			stage.update();
		}
	});
</script>
