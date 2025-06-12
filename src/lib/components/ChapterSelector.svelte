<script>
  import RadioSelector from './RadioSelector.svelte';
  import ChapterTable from './ChapterTable.svelte';
  import Card from './Card.svelte';
  import SummarySection from './SummarySection.svelte';
  import QuestionsList from './QuestionsList.svelte';
  import { selectedGroups, selectedQuestions } from '../stores/questionStore';

  // Mock data - TODO TO REPLACE WITH API DATA LATER 
  const mockChapters = [
    { id: 1, type: 'Chapter', name: 'Algebra', questionCount: 2 },
    { id: 2, type: 'Chapter', name: 'Trigonometry', questionCount: 2 },
    { id: 3, type: 'Chapter', name: 'Geometry', questionCount: 2 },
    { id: 4, type: 'Chapter', name: 'Statistics', questionCount: 2 },
  ];

  const criteriaOptions = [
    { label: 'Chapter', value: 'chapter' },
    { label: 'Topic', value: 'topic' },
    { label: 'Subtopic', value: 'subtopic' }
  ];

  let selectedCriteria = 'chapter';
  let selectedChapters = [];
  let showSummary = false;
  
  //  use when integrating with backend
  async function fetchChapters() {
    try {
      // const response = await fetch('/api/chapters');
      // return await response.json();
      return mockChapters; // Remove this when implementing actual API
    } catch (error) {
      console.error('Error fetching chapters:', error);
      return [];
    }
  }

  let chapters = mockChapters; // Replace with fetchChapters() when implementing API

  function handleCreateGroup() {
    showSummary = true;
    const selectedChapterNames = selectedChapters.map(c => c.name.slice(0, 3)).join('_');
    const totalQuestions = selectedChapters.reduce((sum, c) => sum + c.questionCount, 0);
    
    const newGroup = {
      description: selectedChapterNames,
      availableQuestions: totalQuestions,
      questionsToInsert: Math.min(10, totalQuestions)
    };
    
    selectedGroups.update(groups => [...groups, newGroup]);
    
    // Dispatch event for SummarySection
    const event = new CustomEvent('groupCreated', { detail: newGroup });
    dispatchEvent(event);
  }

  function handleGroupAdded(group) {
    selectedGroups.update(groups => [...groups, group]);
  }
</script>

<Card title="Chapter/ Topic Selection">
  <div class="space-y-6">
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-2">
        Criteria Selector
      </label>
      <RadioSelector
        options={criteriaOptions}
        bind:selected={selectedCriteria}
        name="criteria"
      />
    </div>

    <div>
      <div class="flex justify-between items-center mb-4">
        <h3 class="text-lg font-medium">Available questions ({chapters.length})</h3>
        <button class="text-blue-600 hover:text-blue-800">Select All</button>
      </div>
      
      <ChapterTable
        {chapters}
        bind:selectedChapters
      />
    </div>

    {#if selectedChapters.length > 0}
      <div class="mt-4 flex justify-end">
        <button
          class="bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700"
          on:click={handleCreateGroup}
        >
          Create Group
        </button>
      </div>
    {/if}
  </div>
</Card>

<SummarySection 
  visible={showSummary} 
  onGroupAdded={handleGroupAdded}
  on:groupCreated={e => {
    const summarySection = e.target.querySelector('div');
    if (summarySection) {
      summarySection.handleAddGroup(e.detail);
    }
  }}
/>




<!-- Future API endpoints to implement:

GET /api/questions - Get all questions
Parameters: chapterId, type, difficulty, status
Returns: Question[]

POST /api/groups - Create a new question group
Body: { description: string, questionIds: number[], questionsToInsert: number }
Returns: Group

PUT /api/groups/:id - Update a group
Body: { description?: string, questionsToInsert?: number }
Returns: Group

DELETE /api/groups/:id - Delete a group
Returns: void
-->