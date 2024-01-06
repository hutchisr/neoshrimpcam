<script lang="ts">
  import { Spinner } from "@sveltestrap/sveltestrap";
  // Hack to get around lack of shared export
  import { type TextColor } from "../../node_modules/@sveltestrap/sveltestrap/src/shared";

  export let size = "md";
  export let color = "primary" as TextColor;
</script>

<div class="text-center">
  <Spinner type="grow" {size} {color} />
</div>

<style>
  .text-center {
    text-align: center;
  }
</style>
