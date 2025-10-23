<script lang="ts">
  import type { Snippet } from "svelte";

  interface Props {
    onclick?: () => void;
    label: string;
    disabled?: boolean;
    children: Snippet;
  }

  let {
    onclick = () => {},
    label,
    children,
    disabled = false,
    ...restProps
  }: Props = $props();
</script>

<button
  {onclick}
  {disabled}
  class="disabled:bg-destructive fixed bottom-6 cursor-pointer right-6 w-16 h-16 hover:w-48 group rounded-full bg-primary text-primary-foreground shadow-lg flex items-center justify-center transition-all duration-150 active:scale-95"
  {...restProps}
>
  {#if children}
    {@render children?.()}
  {/if}

  <span class="ml-3 hidden group-hover:block">{label}</span>
</button>
