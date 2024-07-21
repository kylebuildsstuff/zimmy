<script lang="ts">
	import { Frame } from 'zimjs';
	import { Board } from '@zimjs/game';

	new Frame({
		scaling: FIT,
		width: 1024,
		height: 768,
		color: light,
		outerColor: dark,
		ready: (frame, stage, width, height, mobile) => {
			const board = new Board().center();
		}
	});
</script>
