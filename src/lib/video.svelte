<script lang="ts">
  import { onMount, getContext } from "svelte";
  import { browser } from "$app/environment";
  import type { Writable } from "svelte/store";
  import "/node_modules/plyr/dist/plyr.css";
  import Loading from "./loading.svelte";

  let player: Plyr;
  $: duration = 0;
  $: currentTime = 0;
  const error = getContext("error") as Writable<boolean>;
  const playing = getContext("playing") as Writable<boolean>;

  export let src: string;

  onMount(setup);

  async function setup() {
    const Plyr = (await import("plyr")).default;
    const Hls = (await import("hls.js")).default;
    const video = document.querySelector("#video-player");
    player = new Plyr("#video-player", {
      autoplay: true,
      muted: true,
      controls: ["play-large", "fullscreen", "pip", "airplay"]
    });
    if (Hls.isSupported()) {
      // For more Hls.js options, see https://github.com/dailymotion/hls.js
      const hls = new Hls({
        "liveSyncDurationCount": 1,
        "liveMaxLatencyDurationCount": 2.5,
        "maxLiveSyncPlaybackRate": 1,
        "liveDurationInfinity": true
      });
      hls.loadSource(src);
      hls.attachMedia(video as HTMLMediaElement);
    }

    player.on("error", (err) => {
      console.error(err);
      error.set(true);
      playing.set(false);
    });

    player.on("playing", (_event) => {
      error.set(false);
      playing.set(true);
    });

    player.on("timeupdate", (_event) => {
      duration = player.duration;
      currentTime = player.currentTime;
    });

    player.on("seeked", (_event) => {
      player.play();
    });

    player.on("stalled", _event => {
      playing.set(false);
      error.set(true);
      // player.destroy();
      // setup();
    });

    player.volume = 0;
    player.play();
  }

  function onFocus() {
    player.forward(Infinity);
  }
</script>

<svelte:window on:focus={onFocus} />

<video id="video-player" autoplay muted playsinline>
  <source {src} />
  <track kind="captions" />
</video>
{#if !$playing}
  <Loading />
{/if}
{#if typeof location !== "undefined" && location.hash == "#debug"}
  <p>Duration: {duration}s</p>
  <p>Current Time: {currentTime}s</p>
{/if}
