<script>
  import Card from '$lib/components/Card.svelte';
  import { goto } from '$app/navigation';
  
  let papers = [
    {
      questionPaperId: 'QP001',
      eventName: 'Mid Term Exam',
      subjectName: 'Mathematics',
      standard: '10th',
      medium: 'English',
      createdAt: '2023-06-23',
      status: 'Draft',
      isEditable: true // Paper still in review phase
    },
    {
      questionPaperId: 'QP002',
      eventName: 'Final Exam',
      subjectName: 'Science',
      standard: '9th',
      medium: 'English',
      createdAt: '2023-11-10',
      status: 'Published',
      isEditable: false // Paper already generated
    },
    {
      questionPaperId: 'QP003',
      eventName: 'Unit Test 1',
      subjectName: 'English Literature',
      standard: '8th',
      medium: 'Hindi',
      createdAt: '2023-07-05',
      status: 'Draft',
      isEditable: true // Paper still in review phase
    },
    {
      questionPaperId: 'QP004',
      eventName: 'Pre-board Exam',
      subjectName: 'Social Science',
      standard: '10th',
      medium: 'English',
      createdAt: '2023-12-01',
      status: 'Archived',
      isEditable: false // Paper already generated
    },
    {
      questionPaperId: 'QP005',
      eventName: 'Unit Test 2',
      subjectName: 'Physics',
      standard: '12th',
      medium: 'English',
      createdAt: '2023-08-15',
      status: 'Published',
      isEditable: false // Paper already generated
    },
    {
      questionPaperId: 'QP006',
      eventName: 'Quarterly Exam',
      subjectName: 'Chemistry',
      standard: '11th',
      medium: 'Hindi',
      createdAt: '2023-09-20',
      status: 'Draft',
      isEditable: true // Paper still in review phase
    },
    {
      questionPaperId: 'QP007',
      eventName: 'Monthly Test',
      subjectName: 'Biology',
      standard: '9th',
      medium: 'English',
      createdAt: '2023-10-05',
      status: 'Published',
      isEditable: false // Paper already generated
    },
    {
      questionPaperId: 'QP008',
      eventName: 'Annual Exam',
      subjectName: 'Computer Science',
      standard: '12th',
      medium: 'English',
      createdAt: '2023-12-15',
      status: 'Published',
      isEditable: false // Paper already generated
    },
    {
      questionPaperId: 'QP009',
      eventName: 'Class Test',
      subjectName: 'History',
      standard: '8th',
      medium: 'Hindi',
      createdAt: '2023-06-30',
      status: 'Draft',
      isEditable: true // Paper still in review phase
    },
    {
      questionPaperId: 'QP010',
      eventName: 'Weekly Quiz',
      subjectName: 'Geography',
      standard: '7th',
      medium: 'English',
      createdAt: '2023-05-12',
      status: 'Archived',
      isEditable: false // Paper already generated
    }
  ];

  // Search and sort state
  let searchQuery = '';
  let sortField = 'createdAt';
  let sortDirection = 'desc';

  // Reactive declarations for filtering and sorting
  $: filteredPapers = papers
    .filter(paper => 
      Object.entries(paper).some(([key, value]) => 
        String(value).toLowerCase().includes(searchQuery.toLowerCase())
      )
    )
    .sort((a, b) => {
      let comparison = 0;
      if (a[sortField] < b[sortField]) comparison = -1;
      if (a[sortField] > b[sortField]) comparison = 1;
      return sortDirection === 'desc' ? -comparison : comparison;
    });

  function toggleSort(field) {
    if (sortField === field) {
      sortDirection = sortDirection === 'asc' ? 'desc' : 'asc';
    } else {
      sortField = field;
      sortDirection = 'asc';
    }
  }

  function getSortIcon(field) {
    if (sortField !== field) return '↕️';
    return sortDirection === 'asc' ? '↑' : '↓';
  }

  function formatDate(dateString) {
    return new Date(dateString).toLocaleDateString();
  }

  function getStatusStyle(status) {
    switch(status.toLowerCase()) {
      case 'draft':
        return 'bg-yellow-100 text-yellow-800';
      case 'published':
        return 'bg-green-100 text-green-800';
      default:
        return 'bg-gray-100 text-gray-800';
    }
  }

  // Add edit handling function
  function handleEdit(paperId) {
    const paper = papers.find(p => p.questionPaperId === paperId);
    if (paper && paper.isEditable) {
      goto(`/edit/${paperId}`);
    }
  }
</script>

<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
  <Card title="Search Question Papers">
    <div class="space-y-6">
      <!-- Search Bar -->
      <div class="flex items-center space-x-4">
        <div class="flex-1">
          <input
            type="text"
            bind:value={searchQuery}
            placeholder="Search papers..."
            class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        </div>
      </div>

      <!-- Results Table -->
      <div class="overflow-x-auto bg-white rounded-lg shadow">
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
                  scope="col" 
                  class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider {header.key !== 'actions' ? 'cursor-pointer hover:bg-gray-100' : ''}"
                  on:click={() => header.key !== 'actions' && toggleSort(header.key)}
                >
                  <div class="flex items-center space-x-1">
                    <span>{header.label}</span>
                    {#if header.key !== 'actions'}
                      <span class="text-gray-400">{getSortIcon(header.key)}</span>
                    {/if}
                  </div>
                </th>
              {/each}
            </tr>
          </thead>
          <tbody class="bg-white divide-y divide-gray-200">
            {#each filteredPapers as paper (paper.questionPaperId)}
              <tr class="hover:bg-gray-50">
                <td class="px-6 py-4 whitespace-nowrap">{paper.questionPaperId}</td>
                <td class="px-6 py-4 whitespace-nowrap">{paper.eventName}</td>
                <td class="px-6 py-4 whitespace-nowrap">{paper.subjectName}</td>
                <td class="px-6 py-4 whitespace-nowrap">{paper.standard}</td>
                <td class="px-6 py-4 whitespace-nowrap">{paper.medium}</td>
                <td class="px-6 py-4 whitespace-nowrap">{formatDate(paper.createdAt)}</td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <span class="px-2 py-1 text-xs font-medium rounded-full {getStatusStyle(paper.status)}">
                    {paper.status}
                  </span>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  {#if paper.isEditable}
                    <button
                      on:click={() => handleEdit(paper.questionPaperId)}
                      class="text-blue-600 hover:text-blue-800 font-medium"
                    >
                      Edit
                    </button>
                  {:else}
                    <span class="text-gray-400 text-sm">Not Editable</span>
                  {/if}
                </td>
              </tr>
            {/each}
            {#if filteredPapers.length === 0}
              <tr>
                <td colspan="8" class="px-6 py-4 text-center text-gray-500">
                  No papers found matching your search criteria
                </td>
              </tr>
            {/if}
          </tbody>
        </table>
      </div>

      <!-- Results Count -->
      <div class="text-sm text-gray-500">
        Showing {filteredPapers.length} of {papers.length} papers
      </div>
    </div>
  </Card>
</div>