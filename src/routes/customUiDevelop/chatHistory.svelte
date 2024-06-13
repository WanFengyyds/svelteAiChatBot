<script>
    import { ScrollArea } from "$lib/components/ui/scroll-area/index.js";
    import { Separator } from "$lib/components/ui/separator/index.js";
    import { ModeWatcher } from "mode-watcher";

    const tags = Array.from({ length: 50 }).map(
        (_, i, a) => `v1.2.0-beta.${a.length - i}`,
    );
</script>

<ModeWatcher />
<ScrollArea class="h-full w-48 rounded-md border">
    <div class="p-4">
        <h4 class="mb-4 text-sm font-medium leading-none">Tags</h4>
        {#each tags as tag}
            <div class="text-sm">
                {tag}
            </div>
            <Separator class="my-2" />
        {/each}
    </div>
</ScrollArea>
