<script>
  import Card from '$lib/components/Card.svelte';
  import Input from '$lib/components/Input.svelte';
  import RadioGroup from '$lib/components/RadioGroup.svelte';
  import Dropdown from '$lib/components/Dropdown.svelte';
  import DifficultyDistribution from '$lib/components/DifficultyDistribution.svelte';
  import ExamConfig from '$lib/components/ExamConfig.svelte';
  import ChapterSelector from '$lib/components/ChapterSelector.svelte';
  import ActionButtons from '$lib/components/ActionButtons.svelte';
  import SummaryTable from '$lib/components/SummaryTable.svelte';
  import GroupTable from '$lib/components/GroupTable.svelte';
  import ConfigurationSummary from '$lib/components/ConfigurationSummary.svelte';
  import QuestionDifficultyDistribution from '$lib/components/QuestionDifficultyDistribution.svelte';
  import QuestionGroupsPreview from '$lib/components/QuestionGroupsPreview.svelte';
  import ActionBar from '$lib/components/ActionBar.svelte';
  import QuestionsList from '$lib/components/QuestionsList.svelte';
  
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

  let groups = [];
  let groupCounter = 1;
  let allQuestions = []; // Store all available questions

  const examModes = ['Online', 'Offline', 'Hybrid'];
  const classOptions = Array.from({length: 12}, (_, i) => (i + 1).toString());
  const mediumOptions = ['Hindi', 'English', 'Tamil'];
  const subjectOptions = ['Science', 'Maths', 'Physics'];

  let currentView = 'config'; // 'config' | 'review'

  function handleReset() {

    selectedGroups.set([]);
    selectedQuestions.set([]);
    groups = [];
    groupCounter = 1;
  }

  function handleDelete() {
    selectedGroups.set([]);
    selectedQuestions.set([]);
    groups = [];
    groupCounter = 1;
  }

  function handleCreatePaper(event) {
    event.preventDefault();
    currentView = 'review';
  }
  function handleSubmit(event){
    event.preventdefault() ; 
  }
  function handleCreateGroup(groupData) {
    groups = [...groups, {
      description: groupData.description,
      availableQuestions: groupData.availableQuestions,
      questionsToInsert: 0
    }];
    console.log('New group added:', groups); // Debug log
  }
  function handleGroupCreate(event) {
    const { type, items, totalQuestions } = event.detail;
    
    const newGroup = {
      id: groupCounter,
      description: `Group ${groupCounter}`,
      type: type,
      items: items.map(item => ({...item})),
      availableQuestions: totalQuestions,
      questionsToInsert: 10
    };
    
    // Populate questions for the selected items
    allQuestions = items.map((item, index) => ({
      id: index + 1,
      text: `Sample question ${index + 1} from ${item.name}`,
      type: "MCQ",
      marks: 2,
      difficulty: "Medium",
      chapter: item.name,
      isBlacklisted: false
    }));
    
    groups = [...groups, newGroup];
    groupCounter++;
  }

  function handleBack(event) {
    event.preventDefault();
    currentView = 'config';
  }

  function handleGenerate() {
    console.log('Generating papers with config:', {
      examTitle,
      examMode,
      examClass,
      examMedium,
      examSubject,
      totalTime,
      totalQuestions,
      numberOfSets,
      numberOfVersions,
      groups
    });
  }
</script>

<div class="max-w-3xl mx-auto px-4 py-8">
  {#if currentView === 'config'}
    <h1 class="text-2xl font-bold font-inter mb-8">Create exam event</h1>

    <form on:submit|preventDefault={handleSubmit}>
      <Card title="Exam details">

   <div>
    <label class="block text-sm font-medium text-gray-700 mb-1">
    Exam title <span class = "text-red-600">*</span>
    </label>
    <input
      type="text"
      required
      class="w-full px-3 py-2 border rounded-md"
      bind:value={examTitle}
    />
  </div>

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

     <Card title="Exam paper configuration">
      <ExamConfig
        bind:totalTime
        bind:totalQuestions
        bind:numberOfSets
        bind:numberOfVersions
      />
    </Card>


    

    <Card title="Define Difficulty Level Distribution">
      <DifficultyDistribution />
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

   
    <ChapterSelector on:createGroup={handleGroupCreate} />
  </form>

  <!-- {#if groups.length > 0}
    <div class="mt-8">
      <GroupTable
        {groups}
        onUpdate={(index, field, value) => {
          groups[index][field] = value;
          groups = [...groups];
        }}
        onDelete={(index) => {
          groups = groups.filter((_, i) => i !== index);
        }}
      />
    </div>

    <QuestionsList 
      {groups}
      questions={allQuestions}
      on:updateGroups={(e) => groups = e.detail}
      on:updateQuestions={(e) => allQuestions = e.detail}
    />
  {/if} -->
  
{#if groups.length > 0}
  <QuestionsList 
    {groups}
    questions={allQuestions}
    on:updateGroups={(e) => {
      groups = e.detail;
    }}
    on:updateQuestions={(e) => {
      allQuestions = e.detail;
    }}
  />
{/if}


  <div class="mt-8 mb-8">
    <ActionButtons 
      
      onReset={handleReset}
      onDelete={handleDelete}
      onCreatePaper={handleCreatePaper}
    />
  </div>
  {:else}
    <!-- Review View -->
    <div class="space-y-8">
      <Card title="Review Configuration">
        <ConfigurationSummary 
          examTitle={examTitle}
          examMode={examMode}
          examClass={examClass}
          examMedium={examMedium}
          examSubject={examSubject}
          {totalTime}
          {totalQuestions}
          {numberOfSets}
          {numberOfVersions}
        />
      </Card>

      <Card title="Question Distribution">
        <QuestionDifficultyDistribution {groups} />
      </Card>

      <Card title="Question Groups">
        <QuestionGroupsPreview {groups} />
      </Card>

      <ActionBar
        {totalTime}
        {numberOfSets}
        {numberOfVersions}
        onCancel={handleBack}
        onReset={handleReset}
        onGenerate={handleGenerate}
      />
    </div>
  {/if}
</div>