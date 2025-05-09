<script>
    import 'prismjs';
    import 'prismjs/themes/prism-tomorrow.css';
  
    import 'prismjs/plugins/line-numbers/prism-line-numbers.js';
    import 'prismjs/plugins/line-numbers/prism-line-numbers.css';
    // Supported languages
    import 'prismjs/components/prism-javascript.js';
    import 'prismjs/components/prism-typescript.js';
    import 'prismjs/components/prism-python.js';
    import 'prismjs/components/prism-markup.js'; // HTML
    import 'prismjs/components/prism-css.js';
    import 'prismjs/components/prism-json.js';
    import 'prismjs/components/prism-rust.js';
    import 'prismjs/components/prism-go.js';
    import 'prismjs/components/prism-bash.js'; // Shell
  
    export let file;
  
    let codeContent = '';
    let language = 'plaintext';
    let codeElement;
  
    // Extension to Prism language mapping
    function detectLang(ext) {
      const map = {
        js: 'javascript',
        ts: 'typescript',
        py: 'python',
        html: 'markup',
        css: 'css',
        json: 'json',
        md: 'markdown',
        rs: 'rust',
        go: 'go',
        sh: 'bash',
        bash: 'bash',
        txt: 'plaintext',
      };
      return map[ext] || 'plaintext';
    }
  
    // This runs when `file` changes (Svelte 5 reactive block)
    $: if (file) {
      handleFile(file);
    }
  
    function handleFile(file) {
      const parts = file.name.split('.');
      const ext = parts.length > 1 ? parts.pop().toLowerCase() : ''; // Handle no extension
      language = detectLang(ext);
  
      const reader = new FileReader();
      reader.onload = (e) => {
        codeContent = e.target.result;
        // Highlight after DOM updates
        queueMicrotask(() => {
          if (codeElement) {
            Prism.highlightElement(codeElement);
          }
        });
      };
      reader.readAsText(file);
    }
  </script>
  
  <style>
    .code-container {
      max-width: 100%;
      overflow-x: auto;
      border-radius: 8px;
      margin: auto;
      margin-top: 1rem;
    }
  
    pre {
      padding: 1rem;
      border-radius: 8px;
      font-size: 0.9rem;
    }
  
    .meta {
      font-size: 0.9rem;
      color: #555;
      margin-top: 0.5rem;
      text-align: center;
    }
  </style>
  
  <!-- Display Code -->
  {#if codeContent}
    <div class="code-container">
<pre class="language-{language} line-numbers">
<code bind:this={codeElement} class="language-{language}">{codeContent}</code>
      </pre>
      <div class="meta">
        <p><strong>Name:</strong> {file.name}</p>
        <p><strong>Size:</strong> {(file.size / 1024).toFixed(2)} KB</p>
        <p><strong>Language:</strong> {language}</p>
      </div>
    </div>
  {/if}
  