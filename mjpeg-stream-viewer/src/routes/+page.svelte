<script lang="ts">
  import Fab from "$lib/components/Fab/Fab.svelte";
  import { Plus } from "@lucide/svelte";
  import Mjpegviewer from "../lib/components/mjpegviewer.svelte";
  let noOfViewers = $state(1);

  function addViewer() {
    noOfViewers += 1;
  }
</script>

<main>
  <div class="flex flex-wrap p-4 gap-2">
    {#each Array(noOfViewers) as _}
      <Mjpegviewer></Mjpegviewer>
    {/each}
  </div>
  <Fab onclick={addViewer} label="Add Stream"><Plus /></Fab>
</main>
