<script lang="ts">
	import '/node_modules/plyr/dist/plyr.css';
	import { onMount } from 'svelte';

	let player: Plyr;

	export let src: string;

	onMount(async () => {
		const Plyr = (await import('plyr')).default;
		const Hls = (await import('hls.js')).default;
		const video = document.querySelector('#video-player');
		player = new Plyr('#video-player', {
			autoplay: true,
			muted: true,
			controls: ['play-large', 'fullscreen', 'pip', 'airplay'],
		});
		if (Hls.isSupported()) {
			// For more Hls.js options, see https://github.com/dailymotion/hls.js
			const hls = new Hls();
			hls.loadSource(src);
			hls.attachMedia(video as HTMLMediaElement);
		}
		player.volume = 0;
		player.forward(Infinity);
		player.play();

		player.on('error', () => player.restart())
	});

	function onFocus() {
		player.forward(Infinity);
		player.play();
	}
</script>

<svelte:window on:focus={onFocus} />

<video id="video-player" autoplay muted>
	<source {src} />
	<track kind="captions" />
</video>
