<script>
    import File from '$lib/compoments/File.svelte';
</script>

<div class="grid h-screen grid-rows-[auto_1fr_auto]">

    <header class="bg-red-500 p-4">(header)</header>

      <div class="grid grid-cols-1 md:grid-cols-[auto_1fr_auto]">

          <aside class="bg-yellow-500 p-4">(sidebar)</aside>

          <main class="p-4 space-y-4">
            <File/>
          </main>

          <aside class="bg-yellow-500 p-4">(sidebar)</aside>
      </div>

    <footer class="bg-blue-500 p-4">(footer)</footer>
  </div>
  