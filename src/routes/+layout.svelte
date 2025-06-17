<script>
  let { children } = $props();
  import "../app.css";
  import Header from "$lib/components/Header.svelte";
    import Footer from '$lib/components/Footer.svelte';


</script>

<div class = "min-h-screen bg-gray-50">
    <Header  />
    <main>
        <slot/>
    </main>
    <Footer />
</div>
<!-- {@render children()} -->