<script>
  import Card from '$lib/components/Card.svelte';
  import { papers } from '$lib/stores/paperStore';
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';
  
  // Search and sort state
  let searchQuery = '';
  let sortField = 'createdAt';
  let sortDirection = 'desc';

  // Subscribe to papers store and log changes
  onMount(() => {
    const unsubscribe = papers.subscribe(value => {
      console.log('Papers store value:', value);
    });

    return unsubscribe;
  });

  // Reactive search and sort
  $: {
    console.log('Current papers store value:', $papers);
    console.log('Current search query:', searchQuery);
    console.log('Current sort field:', sortField);
    console.log('Current sort direction:', sortDirection);
  }

  $: filteredPapers = $papers
    .filter(paper => {
      const matches = Object.values(paper)
        .some(value => 
          String(value).toLowerCase().includes(searchQuery.toLowerCase())
        );
      console.log('Paper filtering:', { paper, matches });
      return matches;
    })
    .sort((a, b) => {
      let comparison = 0;
      if (a[sortField] < b[sortField]) comparison = -1;
      if (a[sortField] > b[sortField]) comparison = 1;
      return sortDirection === 'desc' ? -comparison : comparison;
    });

  $: console.log('Filtered papers:', filteredPapers);

  function handleEdit(paperId) {
    console.log('Editing paper:', paperId);
    if (paperId) {
      goto(`/edit/${paperId}`);
    }
  }

  function toggleSort(field) {
    console.log('Toggling sort:', { currentField: sortField, newField: field, currentDirection: sortDirection });
    if (sortField === field) {
      sortDirection = sortDirection === 'asc' ? 'desc' : 'asc';
    } else {
      sortField = field;
      sortDirection = 'asc';
    }
  }
</script>

<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
  <Card title="Search Question Papers">
    <!-- Search input -->
    <div class="mb-4">
      <input
        type="text"
        bind:value={searchQuery}
        placeholder="Search papers..."
        class="w-full px-4 py-2 border rounded-md"
      />
    </div>

    <!-- Results table -->
    <div class="overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            {#each [
              { key: 'questionPaperId', label: 'Paper ID' },
              { key: 'eventName', label: 'Exam Title' },
              { key: 'subjectName', label: 'Subject' },
              { key: 'standard', label: 'Class' },
              { key: 'medium', label: 'Medium' },
              { key: 'createdAt', label: 'Created At' },
              { key: 'status', label: 'Status' },
              { key: 'actions', label: 'Actions' }
            ] as header}
              <th 
                class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider cursor-pointer"
                on:click={() => header.key !== 'actions' && toggleSort(header.key)}
              >
                {header.label}
                {#if header.key !== 'actions'}
                  <span class="ml-1">{sortField === header.key ? (sortDirection === 'asc' ? '↑' : '↓') : '↕'}</span>
                {/if}
              </th>
            {/each}
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          {#each filteredPapers as paper}
            <tr>
              <td class="px-6 py-4 whitespace-nowrap">{paper.questionPaperId}</td>
              <td class="px-6 py-4 whitespace-nowrap">{paper.eventName}</td>
              <td class="px-6 py-4 whitespace-nowrap">{paper.subjectName}</td>
              <td class="px-6 py-4 whitespace-nowrap">{paper.standard}</td>
              <td class="px-6 py-4 whitespace-nowrap">{paper.medium}</td>
              <td class="px-6 py-4 whitespace-nowrap">
                {new Date(paper.createdAt).toLocaleDateString()}
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <span class="px-2 py-1 text-xs rounded-full {paper.status === 'Draft' ? 'bg-yellow-100 text-yellow-800' : 'bg-green-100 text-green-800'}">
                  {paper.status}
                </span>
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                {#if paper.isEditable}
                  <button
                    class="text-blue-600 hover:text-blue-800"
                    on:click={() => handleEdit(paper.questionPaperId)}
                  >
                    Edit
                  </button>
                {:else}
                  <span class="text-gray-400">Not Editable</span>
                {/if}
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </Card>
</div>