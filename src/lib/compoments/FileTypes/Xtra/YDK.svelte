<script>
    import Card from "../../Card.svelte";
  
    export let file;
  
    let mainDeck = [];
    let extraDeck = [];
    let sideDeck = [];
  
    function parseYDK(content) {
      let lines = content.split("\n").map((line) => line.trim());
      let section = null;
  
      mainDeck = [];
      extraDeck = [];
      sideDeck = [];
  
      for (let line of lines) {
        if (line.startsWith("#main")) section = "main";
        else if (line.startsWith("#extra")) section = "extra";
        else if (line.startsWith("!side")) section = "side";
        else if (line && !line.startsWith("#") && !line.startsWith("!")) {
          if (section === "main") mainDeck.push(line);
          else if (section === "extra") extraDeck.push(line);
          else if (section === "side") sideDeck.push(line);
        }
      }
    }
  
    // Reactively read and parse the file
    $: if (file) {
      console.log(file)
      let reader = new FileReader();
      reader.onload = (e) => {
        parseYDK(e.target.result);
      };
      reader.readAsText(file.file);
    }
  </script>
  
  <style>
    .deck-container {
      perspective: 1000px;
    }
  </style>
  
  <!-- Deck Viewer -->
  <div class="max-w-[95%] mx-auto px-4">
  
    <!-- Main Deck -->
    <h4 class="bg-gray-900 text-white p-2 rounded flex items-center space-x-2 text-sm">
      <span>Main</span>
      <span class="text-gray-400">[{mainDeck.length}]</span>
    </h4>
    <div class="overflow-x-auto">
      <div class="deck-container grid grid-cols-10 gap-4 py-4 justify-center mx-auto">
        {#each mainDeck as cardId}
          <Card cardId={cardId} />
        {/each}
      </div>
    </div>
  
    <!-- Extra Deck -->
    <h4 class="bg-gray-900 text-white p-2 rounded flex items-center space-x-2 text-sm mt-4">
      <span>Extra</span>
      <span class="text-gray-400">[{extraDeck.length}]</span>
    </h4>
    <div class="overflow-x-auto">
      <div class="grid grid-cols-10 gap-4 py-4 justify-center mx-auto">
        {#each extraDeck as cardId}
          <Card cardId={cardId} />
        {/each}
      </div>
    </div>
  
    <!-- Side Deck -->
    <h4 class="bg-gray-900 text-white p-2 rounded flex items-center space-x-2 text-sm mt-4">
      <span>Side</span>
      <span class="text-gray-400">[{sideDeck.length}]</span>
    </h4>
    <div class="overflow-x-auto">
      <div class="grid grid-cols-10 gap-4 py-4 justify-center mx-auto">
        {#each sideDeck as cardId}
          <Card cardId={cardId} />
        {/each}
      </div>
    </div>
  
  </div>
  