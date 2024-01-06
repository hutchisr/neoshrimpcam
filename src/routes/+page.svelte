<script lang="ts">
  import Video from "$lib/video.svelte";
  let logo = "/shrimpicon/apple-icon-76x76.png";
  let src = "https://shrimpcam.app/live/shrimpcam/index.m3u8";
</script>

<h1 class="text-center">Shrimpcam <img src={logo} alt="Shrimp Icon" /></h1>
<Video {src} />

<svelte:head>
  <title>Shrimpcam</title>
</svelte:head>
