<script>
  import { homeDir } from "@tauri-apps/api/path";
  import { Button } from "./components/ui/button";
  import { Separator } from "./components/ui/separator";
  import { currentPath } from "./store";
</script>

<div>
  Recent Files
  <Separator />
  <Button on:click={async ()=>$currentPath = await homeDir()}>Home</Button>
</div>
