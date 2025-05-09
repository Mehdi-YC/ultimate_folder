<script>
    export let file;
    let videoUrl = '';
    let metadata = {};
  
    function handleLoadedMetadata(event) {
      const video = event.target;
      metadata.duration = video.duration.toFixed(2) + " sec";
      metadata.width = video.videoWidth;
      metadata.height = video.videoHeight;
    }
  
    $: if (file) {
      videoUrl = URL.createObjectURL(file.file);
      metadata = {
        name: file.name,
        size: (file.size / 1024).toFixed(2) + " KB",
      };
    }
  </script>
  
  <style>
    .video-container {
      max-width: 100%;
      text-align: center;
    }
    .video-container video {
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
  
  <div class="video-container">
    {#if videoUrl}
      <video src={videoUrl} controls on:loadedmetadata={handleLoadedMetadata}></video>
      <div class="meta">
        <p><strong>Name:</strong> {metadata.name}</p>
        <p><strong>Size:</strong> {metadata.size}</p>
        {#if metadata.duration}
          <p><strong>Duration:</strong> {metadata.duration}</p>
          <p><strong>Resolution:</strong> {metadata.width} x {metadata.height}</p>
        {/if}
      </div>
    {/if}
  </div>
  