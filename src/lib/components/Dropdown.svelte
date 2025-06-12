<script>
  export let label = '';
  export let options = [];
  export let value = '';
  export let required = false;
</script>

<div class="mb-4">
  {#if label}
    <label class="block text-sm font-medium text-gray-700 mb-1">
      {label}
      {#if required}<span class="text-red-500">*</span>{/if}
    </label>
  {/if}
  <select
    bind:value
    class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-1 focus:ring-blue-500"
  >
    <option value="">Select {label}</option>
    {#each options as option}
      <option value={option}>{option}</option>
    {/each}
  </select>
</div>