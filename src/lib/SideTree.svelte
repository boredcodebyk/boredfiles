<script lang="ts">
  import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";
  import { audioDir, documentDir, downloadDir, homeDir, pictureDir, videoDir } from "@tauri-apps/api/path";
  import { Button } from "./components/ui/button";
  import { currentPath } from "./store";

</script>

<ScrollArea class="h-dvh w-48 border-r fixed left-0 bg-background backdrop-blur">
  <div class="p-4">
    <Button
      variant="ghost"
      class="w-full justify-start"
      on:click={async () => ($currentPath = await homeDir())}
    >
      Home
    </Button>
    <Button
      variant="ghost"
      class="w-full justify-start"
      on:click={async () => ($currentPath = await documentDir())}
    >
      Documents
    </Button><Button
      variant="ghost"
      class="w-full justify-start"
      on:click={async () => ($currentPath = await downloadDir())}
    >
      Downloads
    </Button><Button
      variant="ghost"
      class="w-full justify-start"
      on:click={async () => ($currentPath = await audioDir())}
    >
      Music
    </Button><Button
      variant="ghost"
      class="w-full justify-start"
      on:click={async () => ($currentPath = await videoDir())}
    >
      Video
    </Button><Button
      variant="ghost"
      class="w-full justify-start"
      on:click={async () => ($currentPath = await pictureDir  ())}
    >
      Pictures
    </Button>
  </div>
</ScrollArea>
