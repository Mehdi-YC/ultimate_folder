<script>
    export let file;
    let fileName = "";
    let coverUrl = "";
    let showPlaceholder = false;

    $: if (file) {
        fileName = file.name.replace(/\.[^/.]+$/, ""); // remove .gba extension
        coverUrl = buildPlanetEmuUrl(fileName);
        showPlaceholder = false;
    }

    function buildPlanetEmuUrl(name) {
        const urlName = encodeURIComponent(name);
        showPlaceholder = false;
        return `https://www.planetemu.net/data/screenshots/Nintendo%20-%20Game%20Boy%20Advance/${urlName}.png`;
    }

    function handleImgError() {
        showPlaceholder = true;
    }
</script>

<div class="flex flex-col items-center space-y-4 p-4">
    <h2 class="text-xl font-bold">{fileName}</h2>

    {#if !showPlaceholder}
        <img
            src={coverUrl}
            alt="Game Cover"
            on:error={handleImgError}
            class="w-64 h-auto rounded shadow"
        />
    {:else}
        <div
            class="w-64 h-40 bg-gray-300 flex items-center justify-center rounded"
        >
            No Cover Found
        </div>
    {/if}

    <p class="text-sm text-gray-600">{file.size} bytes</p>
</div>
