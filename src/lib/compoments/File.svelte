<script>
    import YDK from "./FileTypes/Xtra/YDK.svelte";
    import IMG from "./FileTypes/common/IMG.svelte";
    import Video from "./FileTypes/common/Video.svelte";
    import Code from "./FileTypes/common/Code.svelte";
    import Gba from "./FileTypes/Xtra/GBA.svelte";
    import Ps2 from "./FileTypes/Xtra/PS2.svelte";
  
    export let file; // The selected file passed as a prop
    let type = "--"; // The file type (determined from the file extension)
    console.log("HERE")
    // Function to determine the file type based on its extension
    function getFileType(file) {
      const ext = file.name.includes(".")
        ? file.name.split(".").pop().toLowerCase()
        : ""; // Detect no extension
  
      if (ext === "ydk") type = "ydk";
      else if (ext === "gba") type = "gba";
      else if (ext === "iso") type = "ps2";
      else if (ext === "nsp") type = "switch";
      else if (["jpg", "jpeg", "png", "gif", "webp"].includes(ext)) type = "img";
      else if (["mp4", "webm", "ogg"].includes(ext)) type = "video";
      else if (
        [
          "js",
          "ts",
          "py",
          "html",
          "css",
          "json",
          "md",
          "rs",
          "go",
          "sh",
          "bash",
          "txt",
        ].includes(ext)
      )
        type = "code";
      else type = "unknown";
    }
    console.log("HERE",file,type)
    // Whenever a new file is passed, determine its type
    $: if (file) {
      getFileType(file);
    }
  </script>
  
  {#if type === "ydk"}
    <YDK {file} />
  {:else if type === "img"}
    <IMG {file} />
  {:else if type === "video"}
    <Video {file} />
  {:else if type === "code"}
    <Code {file} />
  {:else if type === "gba"}
    <Gba {file} />
  {:else if type === "ps2"}
    <Ps2 {file} />
  {:else if type === "unknown"}
    <p>Unsupported file type.</p>
  {/if}
  