<script>
  let { children } = $props();
  import "../app.css";
  import Header from "$lib/components/Header.svelte";
  import Footer from '$lib/components/Footer.svelte';
</script>

<div class="min-h-screen bg-gray-50 flex flex-col justify-between">
  <div>
    <Header />
    <main>
      <slot/>
    </main>
  </div>
  <Footer />
</div>