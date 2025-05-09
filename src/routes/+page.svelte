<script>
  import File from "$lib/compoments/File.svelte";

  let files = []; // All files with metadata
  let folders = []; // Unique folder list
  let selectedFolder = null;
  let displayedFiles = []; // Files inside selected folder
  let selectedFile = null; // File selected for preview

  // Handle folder selection (folder picker input)
  function handleFolderSelect(event) {
    const fileArray = Array.from(event.target.files);

    files = fileArray.map(file => {
      const path = file.webkitRelativePath;
      const folder = path.split('/').slice(0, -1).join('/');
      return { name: file.name, type: file.type, path, folder, file };
    });

    // Get unique folders
    folders = [...new Set(files.map(f => f.folder))];

    // Auto-select first folder
    selectedFolder = folders[0];
    filterFiles(selectedFolder);
  }

  // Filter and show files in selected folder
  function filterFiles(folder) {
    selectedFolder = folder;
    displayedFiles = files.filter(f => f.folder === folder);
    selectedFile = null; // Clear preview on folder switch
  }

  function handleFileClick(file) {
    selectedFile = file;
  }
</script>

<div class="grid h-screen grid-rows-[auto_1fr_auto]">
  <header class="p-4 h-[10vh]">Header</header>

  <div class="grid grid-cols-1 md:grid-cols-[auto_1fr_auto]">
    

    <aside class="p-4 overflow-y-auto w-[20vw]">
      <!-- <h3 class="font-bold mb-2">Folders</h3> -->

      <!-- Folder picker -->
      <input
        type="file"
        webkitdirectory
        directory
        multiple
        on:change={handleFolderSelect}
        class="mb-4"
      />

      <!-- Folder List -->
      <ul style="height:70vh;overflow-y:scroll">
        {#each folders as folder}
          <li>
            <button
              on:click={() => filterFiles(folder)}
              class="w-full text-left px-3 py-2 rounded-lg hover:bg-surface-200 transition"
              class:selected={folder === selectedFolder}
            >
              {folder}
            </button>
          </li>
        {/each}
      </ul>
    </aside>

    <main class="p-4 space-y-4 w-[35vw]">
      {#if displayedFiles.length === 0}

      {:else}
        <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4">
          {#each displayedFiles as f (f.path)}
            <div
              on:click={() => handleFileClick(f)}
              class="border rounded p-2 text-center shadow hover:shadow-lg cursor-pointer"
            >
              <div class="truncate">{f.name}</div>
              <div class="text-sm text-gray-500">{Math.round(f.file.size / 1024)} KB</div>
            </div>
          {/each}
        </div>
      {/if}
    </main>

    <aside class="p-4 w-[45vw] border-l">
      {#if selectedFile}
        <h3 class="font-bold mb-2">Preview</h3>
        <div class="preview-container h-[75vh]" style="overflow-y:scroll">
          <File file={selectedFile} />
        </div>
      {:else}
        <p>Select a file to preview.</p>
      {/if}
    </aside>
  </div>

  <footer class="bg-blue-500 p-4">Footer</footer>
</div>

<style>
  button.selected {
    background-color: #ddd;
  }
</style>
