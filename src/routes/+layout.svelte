<script>
  let { children } = $props();
  import "../app.css";
  import Header from "$lib/components/Header.svelte";

</script>

<div class = "min-h-screen bg-gray-50">
    <Header  />
    <main>
        <slot/>
    </main>

</div>
<!-- {@render children()} -->