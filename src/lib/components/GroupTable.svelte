<!-- group formed after selecting the chapters/topics/subtopics -->

<script>
  export let groups = [];
  export let onUpdate;
  export let onDelete;
</script>

<div class="overflow-x-auto shadow ring-1 ring-black ring-opacity-5 rounded-lg">
  <table class="min-w-full divide-y divide-gray-200">
    <thead class="bg-gray-50">
      <tr>
        <th class="px-6 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Description</th>
        <th class="px-4 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Type</th>
        <th class="px-4 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Selected Items</th>
        <th class="px-4 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Available Questions</th>
        <th class="px-4 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Questions to Insert</th>
        <th class="px-4 py-4 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
      </tr>
    </thead>
    <tbody class="bg-white divide-y divide-gray-200">
      {#each groups as group, index}
        <tr class="hover:bg-gray-50 transition-colors">
          <td class="px-6 py-4">
            <div class="text-sm font-medium text-gray-900">{group.description}</div>
          </td>
          <td class="px-4 py-4">
            <span class="px-2.5 py-0.5 inline-flex text-xs leading-5 font-semibold rounded-full bg-blue-100 text-blue-800">
              {group.type}
            </span>
          </td>
          <td class="px-4 py-4">
            <div class="flex flex-wrap gap-1.5">
              {#each group.items as item}
                <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-gray-100 text-gray-800">
                  {item.name}
                </span>
              {/each}
            </div>
          </td>
          <td class="px-4 py-4 text-sm text-gray-900">
            {group.availableQuestions}
          </td>
          <td class="px-4 py-4">
            <input
              type="number"
              min="0"
              max={group.availableQuestions}
              class="block w-16 rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500 sm:text-sm"
              value={group.questionsToInsert}
              on:input={(e) => onUpdate(index, 'questionsToInsert', parseInt(e.target.value))}
            />
          </td>
          <td class="px-4 py-4">
            <button
              class="text-red-600 hover:text-red-900 transition-colors focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2 rounded-md p-1.5"
              on:click={() => onDelete(index)}
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
              </svg>
            </button>
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</div>

{#if groups.length === 0}
  <div class="text-center py-12">
    <svg class="mx-auto h-12 w-12 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 13h6m-3-3v6m-9 1V7a2 2 0 012-2h6l2 2h6a2 2 0 012 2v8a2 2 0 01-2 2H5a2 2 0 01-2-2z" />
    </svg>
    <h3 class="mt-2 text-sm font-medium text-gray-900">No groups created</h3>
    <p class="mt-1 text-sm text-gray-500">Create a group by selecting chapters or topics.</p>
  </div>
{/if}