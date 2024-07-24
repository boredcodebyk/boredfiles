<script lang="ts">
  import {
    readDir,
  } from "@tauri-apps/plugin-fs";
  import { currentPath } from "$lib/store";
  import { resolve } from "@tauri-apps/api/path";
  import { buttonVariants } from "./components/ui/button";
  import { File, Folder, Image } from "lucide-svelte";
  import { invoke } from "@tauri-apps/api/core";
  $: path = $currentPath;

  async function updatePath(name: string) {
    $currentPath = await resolve($currentPath, name);
  }

  function isImage(name: string) {
    let check =
      name.endsWith(".png") || name.endsWith(".jpg") || name.endsWith(".jpeg");
    return check;
  }
</script>

<ul>
  {#await readDir($currentPath)}
    ...
  {:then entries}
    {#each entries.sort((a, b) => {
      if (a.isDirectory && (b.isFile || b.isSymlink)) return -1;
      if ((a.isFile || a.isSymlink) && b.isDirectory) return 1;
      return 0;
    }) as entry}
      {#if entry.isDirectory}
        <li>
          <button
            on:dblclick={async () => await updatePath(entry.name)}
            class="w-full !justify-start {buttonVariants({
              variant: 'ghost',
            })}"
            class:hidden={entry.name.startsWith(".")}
          >
            <Folder class="mr-2 h-4 w-4" />
            {entry.name}
          </button>
        </li>
      {/if}
      {#if entry.isFile}
        <li>
          <button
            class="w-full !justify-start {buttonVariants({
              variant: 'ghost',
            })}"
            class:hidden={entry.name.startsWith(".")}
            on:dblclick={async () =>
              await invoke("open_file", {
                path: await resolve($currentPath, entry.name),
              })}
          >
            {#if isImage(entry.name)}
              <Image class="mr-2 h-4 w-4" />
            {:else}
              <File class="mr-2 h-4 w-4" />
            {/if}
            {entry.name}
          </button>
        </li>
      {/if}

      {#if entry.isSymlink}
        <li>
          <button
            class="w-full flex !justify-start items-center {buttonVariants({
              variant: 'ghost',
            })}"
            class:hidden={entry.name.startsWith(".")}
          >
            {entry.name}
          </button>
        </li>
      {/if}
    {/each}
  {/await}
</ul>
