<!-- group formed after selecting the chapters/topics/subtopics -->

<script>
  export let groups = [];
  export let onDelete;
  export let onUpdate;

  function deleteGroup(event, index) {
    // Dispatch delete event to parent instead of modifying groups directly
    event.preventDefault() ; 
    if (typeof onDelete === 'function') {
      onDelete(index);
    }
  }
</script>

<div class="bg-white rounded-lg border p-6">
  <h2 class="text-xl font-semibold mb-4">Question Groups</h2>
  
  <div class="overflow-x-auto">
    <table class="min-w-full">
      <thead class="border-b">
        <tr class="bg-gray-50">
          <th class="text-left px-4 py-3">Group number</th>
          <th class="text-left px-4 py-3">Group description</th>
          <th class="text-left px-4 py-3">Available questions</th>
          <th class="text-left px-4 py-3">Questions to insert</th>
          <th class="text-left px-4 py-3">Actions</th>
        </tr>
      </thead>
      <tbody>
        {#each groups as group, index}
          <tr class="border-b">
            <td class="px-4 py-3">{index + 1}</td>
            <td class="px-4 py-3">
              <input
                type="text"
                class="border rounded px-2 py-1 w-full"
                value={group.description}
                on:input={(e) => onUpdate(index, 'description', e.target.value)}
              />
            </td>
            <td class="px-4 py-3">{group.availableQuestions}</td>
            <td class="px-4 py-3">
              <input
                type="number"
                class="border rounded px-2 py-1 w-20"
                value={group.questionsToInsert}
                min="1"
                max={group.availableQuestions}
                on:input={(e) => onUpdate(index, 'questionsToInsert', parseInt(e.target.value))}
              />
            </td>
            <td class="px-4 py-3">
              <button
                class="text-red-600 hover:text-red-800"
                on:click={(e) => deleteGroup(e, index)}
                
              >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
                </svg>
              </button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>