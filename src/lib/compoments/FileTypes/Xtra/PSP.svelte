<script>
    export let file;
  
    let gameName = '';
    let coverUrl = '';
  
    // When file changes, process it
    $: if (file) {
      gameName = file.name.replace(/\.[^/.]+$/, ""); // remove extension
      coverUrl = getCoverUrl(gameName);
    }
  
    function getCoverUrl(name) {
      // URL encode for Coolrom
      const encodedName = encodeURIComponent(name);
      return `https://coolrom.com.au/screenshots/psp/${encodedName}.jpg`;
    }
  </script>
  
  <div class="p-4">
    <h2 class="text-xl font-bold mb-2">{gameName}</h2>
  
    <img src={coverUrl} alt="Cover" class="w-48 rounded shadow" on:error={() => coverUrl = 'https://via.placeholder.com/300x400?text=No+Cover'} />
  </div>
  