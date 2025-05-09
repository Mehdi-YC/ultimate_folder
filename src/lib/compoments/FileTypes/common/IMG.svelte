<script>
    export let file;
    let imageUrl = '';
    let metadata = {};
  
    $: if (file) {
      imageUrl = URL.createObjectURL(file.file);
      metadata = {
        name: file.name,
        size: (file.size / 1024).toFixed(2) + " KB",
      };
  
      // Get dimensions
      const img = new Image();
      img.onload = () => {
        metadata.width = img.width;
        metadata.height = img.height;
      };
      img.src = imageUrl;
    }
  </script>
  
  <style>
    .img-container {
      max-width: 100%;
      text-align: center;
    }
    .img-container img {
      max-width: 100%;
      border-radius: 8px;
      margin-bottom: 1rem;
    }
    .meta {
      font-size: 0.9rem;
      color: #555;
      text-align: center;
    }
  </style>
  
  <div class="img-container">
    {#if imageUrl}
      <img src={imageUrl} alt="Uploaded Image" />
      <div class="meta">
        <p><strong>Name:</strong> {metadata.name}</p>
        <p><strong>Size:</strong> {metadata.size}</p>
        {#if metadata.width}
          <p><strong>Dimensions:</strong> {metadata.width} x {metadata.height}</p>
        {/if}
      </div>
    {/if}
  </div>
  