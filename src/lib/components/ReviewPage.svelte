<script>
  import Card from './Card.svelte';
  import DifficultyDistribution from './DifficultyDistribution.svelte';
  import QuestionPreview from './QuestionPreview.svelte';
  
  // Props
  export let examTitle = '';
  export let examMode = '';
  export let examClass = '';
  export let examMedium = '';
  export let examSubject = '';
  export let totalTime = 0;
  export let totalQuestions = 0;
  export let numberOfSets = 1;
  export let numberOfVersions = 1;
  export let groups = [];
  export let questions = [];
   export let easy = '';
  export let medium = '';
  export let hard = '';
  export let isReviewPageEnabled = true;
  
  //  // Initialize examData with default values
  //   let examData = {
  //       easy: 0,
  //       medium: 0,
  //       hard: 0,
  //       // Add other exam data properties as needed
  //   };

  
  
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  let difficultyValid = true;
</script>

<div class="space-y-8">
  <!-- Basic Configuration -->
  <!-- Exam Configuration Card -->
<Card>
  <div class="space-y-6">
    <!-- Header -->
    <div class="flex items-center justify-between border-b pb-4">
      <h2 class="text-lg font-semibold text-gray-900">Exam Configuration</h2>
      <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-blue-100 text-blue-800">
        {examMode}
      </span>
    </div>

    <!-- Content Grid -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <!-- Basic Details -->
      <div class="space-y-4">
        <h3 class="text-sm font-medium text-gray-500 uppercase tracking-wider">Basic Details</h3>
        <div class="space-y-3">
          <div class="flex flex-col">
            <span class="text-sm text-gray-500">Title</span>
            <span class="text-sm font-medium text-gray-900">{examTitle}</span>
          </div>
          <div class="flex flex-col">
            <span class="text-sm text-gray-500">Class</span>
            <span class="text-sm font-medium text-gray-900">{examClass}</span>
          </div>
          <div class="flex flex-col">
            <span class="text-sm text-gray-500">Subject</span>
            <span class="text-sm font-medium text-gray-900">{examSubject}</span>
          </div>
        </div>
      </div>

      <!-- Time & Questions -->
      <div class="space-y-4">
        <h3 class="text-sm font-medium text-gray-500 uppercase tracking-wider">Paper Details</h3>
        <div class="space-y-3">
          <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
            <div class="flex flex-col">
              <span class="text-sm text-gray-500">Duration</span>
              <span class="text-sm font-medium text-gray-900">{totalTime} mins</span>
            </div>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-12a1 1 0 10-2 0v4a1 1 0 00.293.707l2.828 2.829a1 1 0 101.415-1.415L11 9.586V6z" clip-rule="evenodd"/>
            </svg>
          </div>
          <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
            <div class="flex flex-col">
              <span class="text-sm text-gray-500">Questions</span>
              <span class="text-sm font-medium text-gray-900">{totalQuestions}</span>
            </div>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-8-3a1 1 0 00-.867.5 1 1 0 11-1.731-1A3 3 0 0113 8a3.001 3.001 0 01-2 2.83V11a1 1 0 11-2 0v-1a1 1 0 011-1 1 1 0 100-2zm0 8a1 1 0 100-2 1 1 0 000 2z" clip-rule="evenodd"/>
            </svg>
          </div>
        </div>
      </div>

      <!-- Version Details -->
      <div class="space-y-4">
        <h3 class="text-sm font-medium text-gray-500 uppercase tracking-wider">Version Details</h3>
        <div class="space-y-3">
          <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
            <div class="flex flex-col">
              <span class="text-sm text-gray-500">Sets</span>
              <span class="text-sm font-medium text-gray-900">{numberOfSets}</span>
            </div>
            <div class="flex items-center gap-1">
              <span class="text-xs text-gray-400">max 10</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                <path d="M7 3a1 1 0 000 2h6a1 1 0 100-2H7zM4 7a1 1 0 011-1h10a1 1 0 110 2H5a1 1 0 01-1-1zM2 11a2 2 0 012-2h12a2 2 0 012 2v4a2 2 0 01-2 2H4a2 2 0 01-2-2v-4z"/>
              </svg>
            </div>
          </div>
          <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
            <div class="flex flex-col">
              <span class="text-sm text-gray-500">Versions</span>
              <span class="text-sm font-medium text-gray-900">{numberOfVersions}</span>
            </div>
            <div class="flex items-center gap-1">
              <span class="text-xs text-gray-400">max 5</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                <path d="M9 2a2 2 0 00-2 2v8a2 2 0 002 2h6a2 2 0 002-2V6.414A2 2 0 0016.414 5L14 2.586A2 2 0 0012.586 2H9z"/>
                <path d="M3 8a2 2 0 012-2v10h8a2 2 0 01-2 2H5a2 2 0 01-2-2V8z"/>
              </svg>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Footer with total papers info -->
    <div class="pt-4 border-t">
      <div class="flex items-center justify-between">
        <span class="text-sm text-gray-500">Total papers to generate</span>
        <span class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-blue-50 text-blue-700">
          {numberOfSets * numberOfVersions} papers
        </span>
      </div>
    </div>
  </div>
</Card>

  <!-- Difficulty Distribution -->
  <Card title="Question Difficulty Distribution">
    <DifficultyDistribution
      bind:easy
      bind:medium
      bind:hard
      bind:isValid={difficultyValid}
      {isReviewPageEnabled}
    />
  </Card>

  <!-- Groups Preview -->
  <Card title="Selected Groups">
    <div class="overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Description</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Type</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Selected Items</th>
            <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Questions</th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          {#each groups as group}
            <tr>
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                {group.description}
              </td>
              <td class="px-6 py-4 whitespace-nowrap">
                <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-blue-100 text-blue-800">
                  {group.type}
                </span>
              </td>
              <td class="px-6 py-4">
                <div class="flex flex-wrap gap-1">
                  {#each group.items as item}
                    <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-gray-100 text-gray-800">
                      {item.name}
                    </span>
                  {/each}
                </div>
              </td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">
                {group.questionsToInsert}
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </Card>

  <!-- Questions Preview
 <Card title="Available Questions">
  <QuestionPreview {questions} />
</Card> -->


  <!-- Action Buttons -->
  <div class="flex items-center justify-between pt-6 border-t">
    <div class="text-sm text-gray-600">
      {numberOfSets} sets × {numberOfVersions} versions = {numberOfSets * numberOfVersions} total papers
    </div>
    <div class="flex space-x-4">
      <button 
        class="px-4 py-2 text-sm font-medium text-gray-700 bg-white border border-gray-300 rounded-md hover:bg-gray-50"
        on:click={() => dispatch('back')}
      >
        Back
      </button>
      <button 
        class="px-4 py-2 text-sm font-medium text-white bg-blue-600 rounded-md hover:bg-blue-700"
        on:click={() => dispatch('generate')}
      >
        Generate Papers
      </button>
    </div>
  </div>
</div>