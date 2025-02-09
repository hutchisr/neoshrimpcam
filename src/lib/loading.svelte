<script lang="ts">
  import { Progress, type Color } from "@sveltestrap/sveltestrap";
  export let color: Color = "primary";
</script>

<style>
  div {
    margin: 1rem 0;
  }
</style>

<div>
  <Progress {color} value={100} animated={true}>Loading...</Progress>
</div>
