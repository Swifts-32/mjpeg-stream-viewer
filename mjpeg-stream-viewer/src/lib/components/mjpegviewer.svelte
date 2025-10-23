<script lang="ts">
  import { Button } from "./ui/button";
  import { Input } from "./ui/input";
  import { EllipsisVertical, Expand, TriangleAlert } from "@lucide/svelte";
  import Label from "./ui/label/label.svelte";
  import * as Card from "$lib/components/ui/card/index.js";
  import * as Dialog from "$lib/components/ui/dialog/index.js";
  import ComboBox from "./ComboBox/ComboBox.svelte";

  // let streamUrl = $state("http://192.168.1.100:8080");
  let streamUrl = $state("http://192.168.1.200:8081/video/mjpeg");
  let isOpen = $state(false);
  let isStreamValid = $state(false);

  function handleLoad() {
    isStreamValid = true;
  }

  function handleError() {
    isStreamValid = false;
  }
</script>

<Card.Root class="w-fit">
  <Card.Header class="flex justify-between">
    <Card.Title>MJPEG Stream Viewer</Card.Title>
    <div class="flex gap-2">
      <Expand
        class={isStreamValid ? "text-foreground" : "text-muted-foreground"}
        onclick={() => isStreamValid && (isOpen = true)}
      />
      <EllipsisVertical />
    </div>
  </Card.Header>
  <Card.Content class="flex flex-col gap-2">
    <ComboBox
      items={[
        {
          label: "http://192.168.1.200:8081/video/mjpeg",
          value: "http://192.168.1.200:8081/video/mjpeg",
        },
        {
          label: "http://192.168.1.100:8080",
          value: "http://192.168.1.100:8080",
        },
      ]}
      bind:value={streamUrl}
    />

    <Button class="w-full" onclick={() => (isStreamValid = true)}
      >Start Stream</Button
    >

    {#if isStreamValid}
      <img
        class="w-100 rounded-sm"
        src={streamUrl}
        alt="MJPEG stream will appear here"
        onload={handleLoad}
        onerror={handleError}
      />
    {:else}
      <div
        class="bg-muted w-100 h-50 gap-4 rounded-sm flex justify-center place-items-center"
      >
        <TriangleAlert size={50} class="mb-4" /> Stream Not Loading
      </div>
    {/if}
  </Card.Content>
</Card.Root>

<Dialog.Root bind:open={isOpen}>
  <Dialog.Content class="w-5/6 sm:min-w-5/6">
    <Dialog.Header>
      <Dialog.Title>Streaming</Dialog.Title>
      <Dialog.Description>{streamUrl}</Dialog.Description>
    </Dialog.Header>

    <img
      class="rounded-sm"
      src={streamUrl}
      alt="MJPEG stream will appear here"
      onload={handleLoad}
      onerror={handleError}
    />
  </Dialog.Content>
</Dialog.Root>
