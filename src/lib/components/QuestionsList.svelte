<script>
  // Mock data - Replace with API data later
  export let questions = [
    { id: 1, question: 'Questions 1', type: 'Mcq', marks: 2, difficulty: 'Easy', chapter: 'Chapter 1', status: 'Active' },
    { id: 2, question: 'Questions 1', type: 'Short', marks: 2, difficulty: 'Easy', chapter: 'Chapter 1', status: 'Active' },
    { id: 3, question: 'Questions 1', type: 'Short', marks: 2, difficulty: 'Medium', chapter: 'Chapter 2', status: 'Inactive' }
  ];

  let selectedItems = [];

  function toggleSelectAll() {
    if (selectedItems.length === questions.length) {
      selectedItems = [];
    } else {
      selectedItems = [...questions];
    }
  }
</script>

<div class="bg-white rounded-lg shadow">
  <div class="p-4 flex justify-between items-center">
    <h2 class="text-lg font-medium">Available questions ({questions.length})</h2>
    <div class="flex gap-4">
      <button class="text-gray-600 hover:text-gray-800" on:click={() => toggleSelectAll()}>
        {selectedItems.length === questions.length ? 'Deselect All' : 'Select All'}
      </button>
      <button class="text-red-600 hover:text-red-800">Delete</button>
      <button class="text-gray-600 hover:text-gray-800">Filters</button>
    </div>
  </div>

  <div class="overflow-x-auto">
    <table class="min-w-full divide-y divide-gray-200">
      <thead>
        <tr>
          <th class="w-12 px-4 py-3"></th>
          <th class="px-4 py-3 text-left">Question</th>
          <th class="px-4 py-3 text-left">Type</th>
          <th class="px-4 py-3 text-left">Marks</th>
          <th class="px-4 py-3 text-left">Difficulty</th>
          <th class="px-4 py-3 text-left">Chapter</th>
          <th class="px-4 py-3 text-left">Actions</th>
        </tr>
      </thead>
      <tbody class="divide-y divide-gray-200">
        {#each questions as question}
          <tr>
            <td class="px-4 py-3">
              <input 
                type="checkbox"
                bind:group={selectedItems}
                value={question}
                class="h-4 w-4 rounded border-gray-300"
              />
            </td>
            <td class="px-4 py-3">{question.question}</td>
            <td class="px-4 py-3">{question.type}</td>
            <td class="px-4 py-3">{question.marks}</td>
            <td class="px-4 py-3">{question.difficulty}</td>
            <td class="px-4 py-3">{question.chapter}</td>
            <td class="px-4 py-3">
              <div class="flex items-center gap-2">
                <span class={`inline-flex items-center px-2 py-1 rounded-full text-xs
                  ${question.status === 'Active' ? 'bg-green-100 text-green-800' : 'bg-gray-100 text-gray-800'}`}>
                  {question.status}
                </span>
              </div>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</div>