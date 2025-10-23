<script lang="ts">
	import { onMount } from "svelte";
	import { ModeWatcher, setMode } from "mode-watcher";
	import "../app.css";

	let { children } = $props();

	onMount(() => {
		setMode("dark");
	});
</script>

<div class="bg-background size-full min-h-screen">
	{@render children()}
</div>
<ModeWatcher />
