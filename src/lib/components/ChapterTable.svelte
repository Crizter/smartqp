<script>
  export let chapters = [];
  export let selectedChapters = [];

  function toggleChapter(chapter) {
    const index = selectedChapters.findIndex(c => c.id === chapter.id);
    if (index === -1) {
      selectedChapters = [...selectedChapters, chapter];
    } else {
      selectedChapters = selectedChapters.filter(c => c.id !== chapter.id);
    }
  }
</script>

<div class="overflow-x-auto">
  <table class="min-w-full divide-y divide-gray-200">
    <thead>
      <tr class="bg-gray-50">
        <th class="w-12 px-4 py-3"></th>
        <th class="px-4 py-3 text-left text-sm font-medium text-gray-700">Criteria</th>
        <th class="px-4 py-3 text-left text-sm font-medium text-gray-700">Name</th>
        <th class="px-4 py-3 text-left text-sm font-medium text-gray-700">Available questions</th>
        <th class="w-24 px-4 py-3"></th>
      </tr>
    </thead>
    <tbody class="divide-y divide-gray-200">
      {#each chapters as chapter}
        <tr>
          <td class="px-4 py-3">
            <input
              type="checkbox"
              checked={selectedChapters.some(c => c.id === chapter.id)}
              on:change={() => toggleChapter(chapter)}
              class="h-4 w-4 rounded-md border-gray-300"
            />
          </td>
          <td class="px-4 py-3">{chapter.type}</td>
          <td class="px-4 py-3">{chapter.name}</td>
          <td class="px-4 py-3">{chapter.questionCount}</td>
          <td class="px-4 py-3">
            <button
              class={`px-3 py-1 rounded-md text-white ${
                selectedChapters.some(c => c.id === chapter.id)
                  ? 'bg-red-500 hover:bg-red-600'
                  : 'bg-green-500 hover:bg-green-600'
              }`}
              on:click={() => toggleChapter(chapter)}
            >
              {selectedChapters.some(c => c.id === chapter.id) ? 'Delete' : 'Add'}
            </button>
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>