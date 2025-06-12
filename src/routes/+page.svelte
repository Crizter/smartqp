<script>
  import Card from '$lib/components/Card.svelte';
  import Input from '$lib/components/Input.svelte';
  import RadioGroup from '$lib/components/RadioGroup.svelte';
  import Dropdown from '$lib/components/Dropdown.svelte';
  import DifficultyDistribution from '$lib/components/DifficultyDistribution.svelte';
  import ExamConfig from '$lib/components/ExamConfig.svelte';
  import ChapterSelector from '$lib/components/ChapterSelector.svelte';
  import ActionButtons from '$lib/components/ActionButtons.svelte';
  import { selectedGroups, selectedQuestions } from '$lib/stores/questionStore';

  let examTitle = '';
  let examMode = 'Online';
  let examClass = '';
  let examMedium = '';
  let examSubject = '';

  let totalTime = 40;
  let totalQuestions = 40;
  let numberOfSets = 1;
  let numberOfVersions = 1;

  const examModes = ['Online', 'Offline', 'Hybrid'];
  const classOptions = Array.from({length: 12}, (_, i) => (i + 1).toString());
  const mediumOptions = ['Hindi', 'English', 'Tamil'];
  const subjectOptions = ['Science', 'Maths', 'Physics'];

  function handleReset() {
    selectedGroups.set([]);
    selectedQuestions.set([]);
  }

  function handleDelete() {
    selectedGroups.set([]);
    selectedQuestions.set([]);
  }

  function handleCreatePaper() {
    // Implement question paper creation logic
    console.log('Creating question paper...');
  }
</script>

<div class="max-w-3xl mx-auto px-4 py-8 mb-20">
  <h1 class="text-2xl font-bold font-inter mb-8">Create exam event</h1>

  <form>
    <Card title="Exam details">
      <Input 
        label="Exam title" 
        placeholder="Enter exam title"
        required={true}
        bind:value={examTitle}
      />

      <div class="mb-4">
        <label class="block text-sm font-medium text-gray-700 mb-1">
          Exam mode
        </label>
        <RadioGroup options={examModes} bind:selected={examMode} />
      </div>

      <div class="mb-4">
        <label class="block text-sm font-me dium text-gray-700 mb-1">
          Exam type : <span class = "text-gray-500"> MCQ   </span>
        </label>
        <!-- Add exam type selection here -->
      </div>
    </Card>

    <Card title="Class & Subject Selection">
      <div class="grid grid-cols-2 gap-4">
        <Dropdown 
          label="Class"
          options={classOptions}
          required={true}
          bind:value={examClass}
        />
        <Dropdown 
          label="Medium"
          options={mediumOptions}
          required={true}
          bind:value={examMedium}
        />
      </div>

      <Dropdown 
        label="Subject"
        options={subjectOptions}
        bind:value={examSubject}
      />
<!-- in case you want to display the selected subject and class result -->
      <!-- {#if examClass}
        <div class="mt-4">
          <p class="text-sm text-gray-600">Selected class:</p>
          <div class="inline-block bg-blue-100 text-blue-800 px-2 py-1 rounded mt-1">
            {examClass}
          </div>
        </div>
      {/if}

      {#if examSubject}
        <div class="mt-4">
          <p class="text-sm text-gray-600">Selected subject:</p>
          <div class="inline-block bg-blue-100 text-blue-800 px-2 py-1 rounded mt-1">
            {examSubject}
          </div>
        </div>
      {/if} -->
    </Card>

    <Card title="Define Difficulty Level Distribution">
      <DifficultyDistribution />
    </Card>

    <Card title="Exam paper configuration">
      <ExamConfig
        bind:totalTime
        bind:totalQuestions
        bind:numberOfSets
        bind:numberOfVersions
      />
    </Card>

    <ChapterSelector />
  </form>
</div>

<ActionButtons 
  onReset={handleReset}
  onDelete={handleDelete}
  onCreatePaper={handleCreatePaper}
/>