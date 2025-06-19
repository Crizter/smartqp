<script>
  import ExampleTable from '$lib/components/ExampleTable.svelte';

  // Table configuration
  const tableHeadersDisplay = [
    { key: 'id', name: 'ID', width: '10%' },
    { key: 'name', name: 'Student Name', width: '25%' },
    { key: 'class', name: 'Class', width: '15%' },
    { key: 'subjects', name: 'Subjects', width: '25%' },
    { key: 'marks', name: 'Total Marks', width: '15%' },
    { key: 'status', name: 'Status', width: '10%' }
  ];

  // Initial data
  const tableData = Array.from({ length: 20 }, (_, i) => ({
    uuid: `student-${i + 1}`,
    id: i + 1,
    name: `Student ${i + 1}`,
    class: `Class ${Math.floor(Math.random() * 12) + 1}`,
    subjects: ['Mathematics', 'Science', 'English'][Math.floor(Math.random() * 3)],
    marks: Math.floor(Math.random() * 100),
    status: Math.random() > 0.5 ? 'Pass' : 'Fail'
  }));

  // State management
  let searchValue = '';
  let currentPage = 1;
  let entriesPerPage = 5;
  let selectedRows = [];

  // Sort configuration
  let sortAccordingTo = {
    header: null,
    entityType: null,
    sortingOrder: null,
    sortKey: null
  };

  // Custom renderers
  const customRenderers = {
    status: (data) => `
      <span class="px-2 py-1 text-xs font-medium rounded-full 
        ${data.status === 'Pass' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'}">
        ${data.status}
      </span>
    `
  };
</script>

<div class="min-h-screen bg-gray-50 py-8">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="space-y-8">
      <!-- Header -->
      <div>
        <h1 class="text-2xl font-semibold text-gray-900">Example Table Demo</h1>
        <p class="mt-2 text-sm text-gray-500">
          This is a demonstration of the ExampleTable component with mock student data.
        </p>
      </div>

      <!-- Table -->
      <div class="bg-white rounded-lg shadow">
        <div class="p-6">
          <ExampleTable
            {tableHeadersDisplay}
            {tableData}
            {customRenderers}
            {sortAccordingTo}
            bind:currentPage
            {entriesPerPage}
            bind:searchValue
            bind:selectedRows
            showPagination={true}
            tableStyle="primary"
            bulkSelect={true}
            notFoundMessage="No student records found"
          />
        </div>
      </div>
    </div>
  </div>
</div>