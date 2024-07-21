<script lang="ts">
	import { Frame, Pic } from 'zimjs';
	import { Board } from '@zimjs/game';
	import block from '$lib/assets/blocks_1-45.png';

	new Frame({
		scaling: FIT,
		width: 1024,
		height: 768,
		color: light,
		outerColor: dark,
		ready: (frame, stage, width, height, mobile) => {
			const customTile = new Pic(block);

			const board = new Board({
				icon: customTile,
				isometric: true
			}).center();
		}
	});
</script>
