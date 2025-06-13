<script>
  import { goto } from '$app/navigation';
  
  export let onReset = () => {};
  export let onDelete = () => {};

  function handleCreatePaper() {
    goto('/review');
  }
</script>

<div class="fixed bottom-0 left-0 right-0 bg-white border-t shadow-lg p-4">
  <div class="max-w-3xl mx-auto flex justify-between items-center">
    <div class="flex gap-4">
      <button
        class="px-6 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50"
        on:click={onReset}
      >
        Reset
      </button>
      <button
        class="px-6 py-2 border border-red-300 text-red-600 rounded-md hover:bg-red-50"
        on:click={onDelete}
      >
        Delete
      </button>
    </div>
    
    <button
      class="px-6 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700"
      on:click={handleCreatePaper}
    >
      Create question paper
    </button>
  </div>
</div>