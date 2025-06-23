<script>
  import { page } from '$app/stores';
  import ExamDetailsForm from '$lib/components/ExamDetailsForm.svelte';
  import ExamConfig from '$lib/components/ExamConfig.svelte';
  import DifficultyDistribution from '$lib/components/DifficultyDistribution.svelte';
  import ClassSubjectSelector from '$lib/components/ClassSubjectSelector.svelte';
  import ChapterSelector from '$lib/components/ChapterSelector.svelte';
  import Card from '$lib/components/Card.svelte';

  // Get the paper ID from the URL
  $: paperId = $page.params.id;

  // Initialize form data (replace with actual paper data fetch)
  let examData = {
    examTitle: '',
    examMode: 'Online',
    examClass: '',
    examMedium: '',
    examSubject: '',
    totalTime: 40,
    totalQuestions: 40,
    numberOfSets: 1,
    numberOfVersions: 1,
    easy: 40,
    medium: 40,
    hard: 20,
    groups: []
  };

  // Form validation states
  let examDetailsValid = true;
  let classSubjectValid = true;
  let examConfigValid = true;
  let difficultyValid = true;

  function handleSave() {
    // Implement save functionality
    console.log('Saving changes:', examData);
  }
</script>

<div class="max-w-3xl mx-auto px-4 py-8">
  <div class="flex justify-between items-center mb-8">
    <h1 class="text-2xl font-bold">Edit Question Paper</h1>
    <button
      on:click={handleSave}
      class="bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700"
    >
      Save Changes
    </button>
  </div>

  <form class="space-y-6">
    <Card title="Exam Details">
      <ExamDetailsForm
        bind:examTitle={examData.examTitle}
        bind:examMode={examData.examMode}
        bind:isValid={examDetailsValid}
      />
    </Card>

    <Card title="Exam Configuration">
      <ExamConfig
        bind:totalTime={examData.totalTime}
        bind:totalQuestions={examData.totalQuestions}
        bind:numberOfSets={examData.numberOfSets}
        bind:numberOfVersions={examData.numberOfVersions}
        groups={examData.groups}
      />
    </Card>

    <Card title="Difficulty Distribution">
      <DifficultyDistribution
        bind:easy={examData.easy}
        bind:medium={examData.medium}
        bind:hard={examData.hard}
        bind:isValid={difficultyValid}
      />
    </Card>

    <ClassSubjectSelector
      bind:examClass={examData.examClass}
      bind:examMedium={examData.examMedium}
      bind:examSubject={examData.examSubject}
      bind:isValid={classSubjectValid}
    />

    <ChapterSelector
      on:createGroup={(event) => {
        examData.groups = [...examData.groups, event.detail];
      }}
    />
  </form>
</div>