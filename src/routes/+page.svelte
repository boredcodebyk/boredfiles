<script lang="ts">
  import BreadcrumbPath from "$lib/BreadcrumbPath.svelte";
  import FolderView from "$lib/FolderView.svelte";
  import Home from "$lib/Home.svelte";
  import { currentPath } from "$lib/store";
  import { invoke } from "@tauri-apps/api/core";

</script>

<div class="block border-t h-dvh ml-48">
  <BreadcrumbPath />
  <div class="mt-16 px-4 py-6 lg:px-8">
    {#if $currentPath === ""}
       <Home/>
    {:else}
        <FolderView/>
    {/if}
  </div>
</div>
