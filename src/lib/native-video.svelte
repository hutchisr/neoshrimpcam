<script lang="ts">
  import { onMount, getContext } from "svelte";
  import { browser } from "$app/environment";
  import type { Writable } from "svelte/store";
  import Loading from "./loading.svelte";

  let video: HTMLVideoElement;
  $: delay = 0;
  const error = getContext("error") as Writable<boolean>;
  const playing = getContext("playing") as Writable<boolean>;

  export let src: string;

  onMount(setup);

  async function setup() {
    const Hls = (await import("hls.js")).default;
    video = document.querySelector("#video-player")!;
    
    if (Hls.isSupported()) {
      const hls = new Hls();
      hls.loadSource(src);
      hls.attachMedia(video!);
    }

    video.addEventListener("canplaythrough", _event => {
      video.play();
    });
    video.addEventListener("stalled", _event => {
      playing.set(false);
      error.set(true);
      video.load();
    });
    video.addEventListener("playing",  _event => {
      error.set(false);
      playing.set(true);
    });
  }

  function onFocus() {
    if (video) {
      video.fastSeek(Infinity);
    }
  }
</script>

<svelte:window on:focus={onFocus} />
<div>
  <video id="video-player" autoplay muted controls>
    <source {src} />
    <track kind="captions" />
  </video>
</div>
{#if !$playing}
  <Loading />
{/if}
{#if delay && location.hash == "#debug"}
  <p>Delay: {delay}s</p>
{/if}

<style>
  #video-player {
    max-width: 1920px;
    max-height: 1080px;
    object-fit: cover;
  }
</style>
