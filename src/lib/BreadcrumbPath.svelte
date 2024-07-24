<script lang="ts">
  import * as Breadcrumb from "$lib/components/ui/breadcrumb/index.js";
  import { currentPath } from "./store";
  import { Button } from "./components/ui/button";
  import { homeDir, resolve } from "@tauri-apps/api/path";
  import ScrollArea from "./components/ui/scroll-area/scroll-area.svelte";
  import { type } from "@tauri-apps/plugin-os";
  import { Home } from "lucide-svelte";

  async function updatePath(index: number) {


    let homePath = await homeDir();

    if ($currentPath.startsWith(homePath)) {
      let nextPath = $currentPath.split(homePath)[1].split("/");
      let crumbs = nextPath.splice(1, nextPath.length);
      let fullPathArray = [homePath, ...crumbs];
      let newPath = fullPathArray.splice(0, index + 1).join("/");
      $currentPath = await resolve(newPath);
    } else {
      let nextPath = $currentPath.split("/");
      let crumbs = nextPath.splice(1, nextPath.length);
      return ["/", ...crumbs];
    }

    // $currentPath = await resolve(newPath);
    // console.log(newPath);
  }


  async function getBreadcrumb(path: string) {
    let homePath = await homeDir();
    if (path.startsWith(homePath)) {
      let nextPath = $currentPath.split(homePath)[1].split("/");
      let crumbs = nextPath.splice(1, nextPath.length);
      return ["Home", ...crumbs];
    } else {
      let nextPath = $currentPath.split("/");
      let crumbs = nextPath.splice(1, nextPath.length);
      return ["/", ...crumbs];
    }
  }
</script>

<ScrollArea orientation="horizontal">
  <div
    class="w-full px-4 flex items-center justify-start mt-0 lg:px-8 fixed border-y top-0 h-16 bg-background whitespace-nowrap"
  >
    {#if type() === "windows"}
      <Breadcrumb.Root>
        <Breadcrumb.List>
          {#each $currentPath.split("/") as item, index}
            <Breadcrumb.Item>
              <Button
                variant="ghost"
                on:click={async () => await updatePath(index)}
              >
                
                {item}
              </Button>
            </Breadcrumb.Item>
            <Breadcrumb.Separator />
          {/each}
        </Breadcrumb.List>
      </Breadcrumb.Root>
    {/if}

    {#if type() === "linux"}
      <Breadcrumb.Root>
        <Breadcrumb.List>
          {#await getBreadcrumb($currentPath) then pathList}
            {#each pathList as item, index}
              <Breadcrumb.Item>
                <Button variant="ghost" on:click={() => updatePath(index)}>
                  {#if index === 0 && item === "Home"}
                  <Home class="mr-2 h-4 w-4" />
                {/if}
                  {item}
                </Button>
              </Breadcrumb.Item>
              <Breadcrumb.Separator />
            {/each}
          {/await}
        </Breadcrumb.List>
      </Breadcrumb.Root>
    {/if}
  </div>
</ScrollArea>
