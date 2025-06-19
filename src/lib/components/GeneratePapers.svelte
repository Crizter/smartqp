<script>
  import Card from './Card.svelte';
  import { generateQuestionPaperId } from '$lib/utils/paperIdGenerator';

  // Props with default values
  export let examTitle = '';
  export let examClass = '';
  export let examMedium = '';
  export let examSubject = '';
  export let numberOfSets = 1;
  export let numberOfVersions = 1;
  export let questions = []; // Default empty array

  // Generate papers reactively
  $: papers = generatePapers();

  function generatePapers() {
    if (!numberOfSets || !numberOfVersions) return [];

    return Array.from({ length: numberOfSets * numberOfVersions }, (_, index) => {
      const setNumber = Math.floor(index / numberOfVersions) + 1;
      const versionNumber = (index % numberOfVersions) + 1;
      
      return {
        questionPaperId: generateQuestionPaperId(setNumber, versionNumber),
        eventId: '100000011',
        eventName: examTitle || 'Untitled Exam',
        subjectName: examSubject || 'Not specified',
        standard: examClass || 'Not specified',
        medium: examMedium || 'Not specified',
      };
    });
  }

  // Handle paper actions
  function handleDownload(paperId) {
    // TODO: Implement download functionality
    console.log('Downloading paper:', paperId);
  }

  function handlePrint(paperId) {
    // TODO: Implement print functionality
    console.log('Printing paper:', paperId);
  }
</script>

<Card title="Generated Question Papers">
  <div class="overflow-x-auto">
    <table class="min-w-full divide-y divide-gray-200">
      <thead class="bg-gray-50">
        <tr>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            Question Paper ID
          </th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            Event ID
          </th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            Event Name
          </th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            Subject Name
          </th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            Standard
          </th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            Medium
          </th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            Actions
          </th>
        </tr>
      </thead>
      <tbody class="bg-white divide-y divide-gray-200">
        {#each papers as paper}
          <tr>
            <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
              {paper.questionPaperId}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
              {paper.eventId}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
              {paper.eventName}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
              {paper.subjectName}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
              {paper.standard}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
              {paper.medium}
            </td>
            <td class="px-6 py-4 whitespace-nowrap text-sm font-medium space-x-2">
              <button
                on:click={() => handleDownload(paper.questionPaperId)}
                class="text-primary hover:text-primary-dark"
              >
                Download
              </button>
              <button
                on:click={() => handlePrint(paper.questionPaperId)}
                class="text-primary hover:text-primary-dark"
              >
                Print
              </button>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
</Card>