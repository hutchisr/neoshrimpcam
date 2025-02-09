<script lang="ts">
  import { setContext } from 'svelte';
  import { writable } from 'svelte/store';
  import Video from "$lib/video.svelte";
  import NativeVideo from '$lib/native-video.svelte';

  const logo = "/shrimpicon/apple-icon-76x76.png";
  const src = "https://shrimpcam.app/live/shrimpcam/index.m3u8";
  const error = writable(false);
  const playing = writable(false);

  setContext('error', error);
  setContext('playing', playing);
</script>

<h1 class="text-center">Shrimpcam <img src={logo} alt="Shrimp Icon" /></h1>
<div class="text-center">
  <Video {src} />
</div>

<svelte:head>
  <title>Shrimpcam</title>
</svelte:head>
