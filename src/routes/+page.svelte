<script>
  import ExamDetailsForm from '$lib/components/ExamDetailsForm.svelte';
  import ExamConfig from '$lib/components/ExamConfig.svelte';
  import DifficultyDistribution from '$lib/components/DifficultyDistribution.svelte';
  import ClassSubjectSelector from '$lib/components/ClassSubjectSelector.svelte';
  import ChapterSelector from '$lib/components/ChapterSelector.svelte';
  import QuestionsList from '$lib/components/QuestionsList.svelte';
  import ActionButtons from '$lib/components/ActionButtons.svelte';
  import Card from '$lib/components/Card.svelte';
  import ConfigurationSummary from '$lib/components/ConfigurationSummary.svelte';
  import QuestionDifficultyDistribution from '$lib/components/QuestionDifficultyDistribution.svelte';
  import QuestionGroupsPreview from '$lib/components/QuestionGroupsPreview.svelte';
  import ActionBar from '$lib/components/ActionBar.svelte';
  import ReviewPage from '$lib/components/ReviewPage.svelte';
  import Stepper from '$lib/components/Stepper.svelte';
  import GeneratePapers from '$lib/components/GeneratePapers.svelte';


  // Exam details state
  let examTitle = '';
  let examMode = 'Online';
  let examClass = '';
  let examMedium = '';
  let examSubject = '';


  
  // Initialize validation states
  let examDetailsValid = true;
  let classSubjectValid = true;
  let examConfigValid = true;
  let difficultyValid = true;
  
  // Initialize validation errors object
  let validationErrors = {
    questions: null,
    versions: null,
    sets: null
  };

  // Exam configuration state
  let totalTime = 40;
  let totalQuestions = 40;
  let numberOfSets = 1;
  let numberOfVersions = 1;

  // Groups and questions state
  let groups = [];
  let allQuestions = [];
  let currentView = 'config';
  let easy = 40;
  let medium = 40;
  let hard = 20;
   


  // prop for the difficulty distribution 
  let isReviewPageEnabled = false;
  $: isReviewPageEnabled = currentView !== 'config';


  function handleGroupCreate(event) {
    const { type, items, totalQuestions } = event.detail;
    
    const newGroup = {
      id: groups.length + 1,
      description: `Group ${groups.length + 1}`,
      type: type,
      items: items.map(item => ({...item})),
      availableQuestions: totalQuestions,
      questionsToInsert: 10
    };
    
    // Populate questions for the selected items
    allQuestions = items.flatMap((item, index) => 
      Array.from({ length: 10 }, (_, i) => ({
        id: allQuestions.length + 1,
        text: `Sample question ${allQuestions.length + 1} from ${item.name}`,
        type: "MCQ",
        marks: 2,
        difficulty: "Medium",
        chapter: item.name,
        isBlacklisted: false
      }))
    );
    
    groups = [...groups, newGroup];
  }

  function handleReset() {
    groups = [];
    allQuestions = [];
  }

  function handleDelete() {
    groups = [];
    allQuestions = [];
  }

  function handleExamConfigValidation(event) {
    examConfigValid = event.detail.isValid;
    // Ensure we always have a valid errors object
    validationErrors = event.detail.errors || {
      questions: null,
      versions: null,
      sets: null
    };
  }

  function handleCreatePaper(event) {
    event.preventDefault();
    if (!examDetailsValid || !classSubjectValid || !examConfigValid) {
      const errors = [];
      if (validationErrors.questions) errors.push(validationErrors.questions);
      if (validationErrors.versions) errors.push(validationErrors.versions);
      if (validationErrors.sets) errors.push(validationErrors.sets);
      
      alert(errors.length > 0 
        ? `Please fix the following errors:\n${errors.join('\n')}`
        : 'Please fix all validation errors before proceeding'
      );
      return;
    }
    if(difficultyValid) { 
      currentView = 'review' ; 
    }
    currentView = 'review';
  }

  function handleExamConfigUpdate(event) {
    totalTime = event.detail.totalTime;
    totalQuestions = event.detail.totalQuestions;
    numberOfSets = event.detail.numberOfSets;
    numberOfVersions = event.detail.numberOfVersions;
  }
  function handleGeneratePapers() { 
    currentView = 'generate' ; 
  }
  
  // Add handleSubmit function
  function handleSubmit(event) {
    event.preventDefault();
    // Add any form submission logic here
    // This function can validate all fields before moving to review
    if (examDetailsValid && classSubjectValid && examConfigValid && difficultyValid) {
      currentView = 'review';
    } else {
      const errors = [];
      if (!examDetailsValid) errors.push('Exam details are incomplete');
      if (!classSubjectValid) errors.push('Class and subject selection is required');
      if (!examConfigValid) errors.push('Exam configuration is invalid');
      if (!difficultyValid) errors.push('Difficulty distribution must total 100%');
      
      alert(errors.join('\n'));
    }
  }

</script>

<div class="max-w-3xl mx-auto px-4 py-8">
  <h1 class="text-2xl font-bold font-inter mb-8">Create exam event</h1>

  <Stepper {currentView} />
  
  {#if currentView === 'config'}
    <form on:submit|preventDefault={handleSubmit}>
      <ExamDetailsForm 
        bind:examTitle 
        bind:examMode
        bind:isValid={examDetailsValid}
      />

      <Card title="Exam paper configuration">
        <ExamConfig
          bind:totalTime
          bind:totalQuestions
          bind:numberOfSets
          bind:numberOfVersions
          {groups}
          on:validate={handleExamConfigValidation}
        />
      </Card>

      <Card title="Define Difficulty Level Distribution">
        <DifficultyDistribution
          bind:easy
          bind:medium
          bind:hard
          bind:isValid={difficultyValid}
          isReviewPageEnabled={false}
        />
      </Card>


      <!-- <Card title="Define Difficulty Level Distribution">
        <DifficultyDistribution 
          bind :
        />
      </Card> -->




      <ClassSubjectSelector 
        bind:examClass
        bind:examMedium
        bind:examSubject
        bind:isValid={classSubjectValid}
      />

      <ChapterSelector on:createGroup={handleGroupCreate} />
    </form>

    {#if groups.length > 0}
      <QuestionsList 
        {groups}
        questions={allQuestions}
        on:updateGroups={(e) => groups = e.detail}
        on:updateQuestions={(e) => allQuestions = e.detail}
      />
    {/if}

    <div class="mt-8 mb-8">
      <ActionButtons 
        onReset={handleReset}
        onDelete={handleDelete}
        onCreatePaper={handleCreatePaper}
      />
    </div>
  {:else if currentView === 'review'}
    <ReviewPage 
      {examTitle}
      {examMode}
      {examClass}
      {examMedium}
      {examSubject}
      {totalTime}
      {totalQuestions}
      {numberOfSets}
      {numberOfVersions}
      {groups}
      {easy}
      {medium}
      {hard}
      isReviewPageEnabled={true}
      questions={allQuestions}
      on:back={() => currentView = 'config'}
      on:generate={handleGeneratePapers}
    />
  {:else if currentView === 'generate'}
    <GeneratePapers
      {examTitle}
      {examClass}
      {examMedium}
      {examSubject}
      {numberOfSets}
      {numberOfVersions}
      questions={allQuestions}
    />
  {/if}
</div>