<script>
  import Card from './Card.svelte';
  import RadioGroup from './RadioGroup.svelte';
  
  export let examTitle = '';
  export let examMode = 'Online';
  export let isValid = false;
  
  const examModes = ['Online', 'Offline', 'Hybrid'];

  $: isValid = examTitle.trim() !== '';
</script>

<Card title="Exam details">
  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1">
      Exam title <span class="text-red-600">*</span>
    </label>
    <input
      type="text"
      required
      class="w-full px-3 py-2 border rounded-md focus:ring-blue-500 focus:border-blue-500 
      {!examTitle.trim() ? 'border-red-300' : 'border-gray-300'}"
      bind:value={examTitle}
      placeholder="Enter exam title"
    />
    {#if !examTitle.trim()}
      <p class="mt-1 text-sm text-red-600">Exam title is required</p>
    {/if}
  </div>

  <div class="mb-4">
    <label class="block text-sm font-medium text-gray-700 mb-1">
      Exam mode
    </label>
    <RadioGroup options={examModes} bind:selected={examMode} />
  </div>

  <div class="mb-4">
    <label class="block text-sm font-medium text-gray-700 mb-1">
      Exam type : <span class="text-gray-500"> MCQ </span>
    </label>
  </div>
</Card>