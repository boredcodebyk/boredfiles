<script lang="ts">
  import "../app.css";
  import SideTree from "$lib/SideTree.svelte";
  
  import { ModeWatcher } from "mode-watcher";
  import * as Command  from "$lib/components/ui/command";
  let open = false

  function handleKeydown(e: KeyboardEvent) {
      if (e.key === "k" && (e.metaKey || e.ctrlKey)) {
        e.preventDefault();
        open = !open;
      }
    }
</script>

<ModeWatcher />
<div class="h-dvh">
  <SideTree />
  <slot />
</div>

<Command.Dialog bind:open>
  <Command.Input placeholder="Type a command or search..." />
  <Command.List>
    <Command.Empty>No results found.</Command.Empty>
    <Command.Group heading="Suggestions">
      <Command.Item>Calendar</Command.Item>
      <Command.Item>Search Emoji</Command.Item>
      <Command.Item>Calculator</Command.Item>
    </Command.Group>
  </Command.List>
</Command.Dialog>

<svelte:window on:keydown={handleKeydown}></svelte:window>