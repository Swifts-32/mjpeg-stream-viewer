<script lang="ts">
  import { Check, ChevronsUpDown, Search } from "@lucide/svelte";
  import { Combobox } from "bits-ui";
  import { Label } from "../ui/label";
  import { Input } from "../ui/input";

  interface Props {
    items: { value: string; label: string }[];
    value: string;
  }
  let { items, value = $bindable() }: Props = $props();
  let searchValue = $state("");
  let isOpen = $state(false);
  let customAnchor = $state<HTMLElement>(null!);

  const filteredItems = $derived.by(() =>
    searchValue === ""
      ? items
      : Array.from(
          items.filter((item) =>
            item.label.toLowerCase().includes(searchValue.toLowerCase()),
          ),
        ),
  );

  function handleInput(e: Event & { currentTarget: HTMLInputElement }) {
    searchValue = e.currentTarget.value;
  }

  function handleOpenChange(open: boolean) {
    if (!open) searchValue = "";
  }
</script>

<div bind:this={customAnchor}>
  <Input
    onclick={() => (isOpen = true)}
    class="col-span-3"
    type="text"
    placeholder="Enter Address"
    {value}
    oninput={handleInput}
  />
</div>

<Combobox.Root type="single" bind:open={isOpen} bind:value>
  <div class="relative">
    <Combobox.Portal>
      <Combobox.Content
        {customAnchor}
        sideOffset={8}
        class="z-50 w-100 rounded-md border bg-background shadow-lg max-h-60 overflow-auto"
      >
        <Combobox.Viewport>
          {#each filteredItems as item (item.value)}
            <Combobox.Item
              value={item.value}
              label={item.label}
              class="flex items-center justify-between px-4 py-2 text-sm hover:bg-muted rounded-md cursor-pointer"
            >
              {#snippet children({ selected })}
                {item.label}
                {#if selected}
                  <Check class="pl-2" />
                {/if}
              {/snippet}
            </Combobox.Item>
          {/each}

          <Combobox.Item
            value={searchValue}
            label={searchValue}
            class="flex items-center justify-between px-4 py-2 text-sm hover:bg-muted rounded-md cursor-pointer"
          >
            {#snippet children({ selected })}
              {searchValue}
              {#if selected}
                <Check class="pl-2" />
              {/if}
            {/snippet}
          </Combobox.Item>
        </Combobox.Viewport>
      </Combobox.Content>
    </Combobox.Portal>
  </div></Combobox.Root
>
