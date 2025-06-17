<script>
  export let totalTime = 40;
  export let totalQuestions = 40;
  export let numberOfSets = 1;
  export let numberOfVersions = 1;

  // Show/hide tooltip state
  let activeTooltip = null;
</script>

<div class="space-y-6">
  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <!-- Total Time Field -->
    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Total Time (minutes)
        <button 
          class="ml-1 text-gray-400 hover:text-gray-600"
          on:mouseenter={() => activeTooltip = 'time'}
          on:mouseleave={() => activeTooltip = null}
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
          </svg>
        </button>
      </label>
      {#if activeTooltip === 'time'}
        <div class="absolute z-10 w-64 p-2 bg-black text-white text-sm rounded-lg shadow-lg -mt-1">
          Total duration of the exam in minutes. This time will be same for all sets and versions.
        </div>
      {/if}
      <input
        type="number"
        bind:value={totalTime}
        min="1"
      class="mt-1 block w-full px-3 py-2 rounded-md border shadow-sm focus:border-blue-500 focus:ring-blue-500"
      />
    </div>

    <!-- Total Questions Field -->
    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Total Questions
        <button 
          class="ml-1 text-gray-400 hover:text-gray-600"
          on:mouseenter={() => activeTooltip = 'questions'}
          on:mouseleave={() => activeTooltip = null}
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
          </svg>
        </button>
      </label>
      {#if activeTooltip === 'questions'}
        <div class="absolute z-10 w-64 p-2 bg-black text-white text-sm rounded-lg shadow-lg -mt-1">
          Number of questions in each exam paper. This will be consistent across all sets and versions.
        </div>
      {/if}
      <input
        type="number"
        bind:value={totalQuestions}
        min="1"
        class="mt-1 block w-full px-3 py-2 rounded-md border shadow-sm focus:border-blue-500 focus:ring-blue-500"
      />
    </div>

    <!-- Number of Sets Field -->
    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Number of Sets
        <button 
          class="ml-1 text-gray-400 hover:text-gray-600"
          on:mouseenter={() => activeTooltip = 'sets'}
          on:mouseleave={() => activeTooltip = null}
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
          </svg>
        </button>
      </label>
      {#if activeTooltip === 'sets'}
        <div class="absolute z-10 w-64 p-2 bg-black text-white text-sm rounded-lg shadow-lg -mt-1">
          Number of different question papers to generate. Each set will have different questions but same difficulty level.
        </div>
      {/if}
      <input
        type="number"
        bind:value={numberOfSets}
        min="1"
        class="mt-1 block w-full px-3 py-2 rounded-md border shadow-sm focus:border-blue-500 focus:ring-blue-500"
      />
    </div>

    <!-- Number of Versions Field -->
    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Number of Versions
        <button 
          class="ml-1 text-gray-400 hover:text-gray-600"
          on:mouseenter={() => activeTooltip = 'versions'}
          on:mouseleave={() => activeTooltip = null}
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 inline" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
          </svg>
        </button>
      </label>
      {#if activeTooltip === 'versions'}
        <div class="absolute z-10 w-64 p-2 bg-black text-white text-sm rounded-lg shadow-lg -mt-1">
          Number of variations for each set. Versions have the same questions as their parent set but in different order.
        </div>
      {/if}
      <input
        type="number"
        bind:value={numberOfVersions}
        min="1"
       class="mt-1 block w-full px-3 py-2 rounded-md border shadow-sm focus:border-blue-500 focus:ring-blue-500"
      />
    </div>
  </div>

  <div class="bg-blue-50 border-l-4 border-blue-400 p-4 mt-4">
    <div class="flex">
      <div class="flex-shrink-0">
        <svg class="h-5 w-5 text-blue-400" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
        </svg>
      </div>
      <div class="ml-3">
        <p class="text-sm text-blue-700">
          You will generate {numberOfSets} different question paper(s) with {numberOfVersions} version(s) each. 
          Each paper will contain {totalQuestions} questions and should be completed in {totalTime} minutes.
        </p>
      </div>
    </div>
  </div>
</div>