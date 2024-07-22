<script lang="ts">
	import { Frame, Pic, Triangle, Container, Shape, Rectangle } from 'zimjs';

	var frame = new Frame({
		scaling: FIT,
		width: 1024,
		height: 768,
		color: light,
		outerColor: dark
	});
	frame.on('ready', function () {
		var stage = frame.stage;
		var stageW = frame.width;
		var stageH = frame.height;

		var tileWidth = 50;
		var tileHeight = 25;
		var gridWidth = 10;
		var gridHeight = 10;

		var isoGroup = new Container().addTo(stage).pos(stageW / 2, 100);

		function toIso(x, y) {
			return {
				x: ((x - y) * tileWidth) / 2,
				y: ((x + y) * tileHeight) / 2
			};
		}

		// Create isometric grid
		for (var y = 0; y < gridHeight; y++) {
			for (var x = 0; x < gridWidth; x++) {
				var iso = toIso(x, y);
				var tile = new Shape().graphics
					.f(frame.color.lighter(20))
					.mt(0, -tileHeight / 2)
					.lt(tileWidth / 2, 0)
					.lt(0, tileHeight / 2)
					.lt(-tileWidth / 2, 0)
					.cp();
				tile.pos(iso.x, iso.y).addTo(isoGroup);
			}
		}

		// Add interactable elements
		var house = new Rectangle(tileWidth, tileHeight * 2, 'red').centerReg().addTo(isoGroup);
		var houseIso = toIso(3, 3);
		house.pos(houseIso.x, houseIso.y - tileHeight / 2);

		var tree = new Triangle(tileWidth, tileHeight * 2, 'green').centerReg().addTo(isoGroup);
		var treeIso = toIso(6, 4);
		tree.pos(treeIso.x, treeIso.y - tileHeight / 2);

		// Add interactivity
		house.on('click', function () {
			zog('You clicked the house!');
		});

		tree.on('click', function () {
			zog('You clicked the tree!');
		});

		stage.update();
	});
</script>
