<script>
  import { createEventDispatcher } from 'svelte';
  import Card from './Card.svelte';
  import GroupTable from './GroupTable.svelte';
  import QuestionTable from './QuestionTable.svelte';

  const dispatch = createEventDispatcher();

  export let groups = [];
  export let questions = [];
  let activeView = 'groups'; // 'groups' or 'questions'

  function handleUpdateGroup(index, field, value) {
    const updatedGroups = [...groups];
    updatedGroups[index][field] = value;
    dispatch('updateGroups', updatedGroups);
  }

  function handleDeleteGroup(index) {
    const updatedGroups = groups.filter((_, i) => i !== index);
    dispatch('updateGroups', updatedGroups);
  }

  function handleQuestionsUpdate(updatedQuestions) {
    dispatch('updateQuestions', updatedQuestions);
  }
</script>

<div class="space-y-4">
  <!-- View Switcher -->
  <div class="flex border-b">
    <button
      class={`px-4 py-2 font-medium text-sm ${
        activeView === 'groups' 
          ? 'border-b-2 border-blue-500 text-blue-600' 
          : 'text-gray-500 hover:text-gray-700'
      }`}
      on:click={() => activeView = 'groups'}
    >
      Groups ({groups.length})
    </button>
    <button
      class={`px-4 py-2 font-medium text-sm ${
        activeView === 'questions' 
          ? 'border-b-2 border-blue-500 text-blue-600' 
          : 'text-gray-500 hover:text-gray-700'
      }`}
      on:click={() => activeView = 'questions'}
    >
      Available Questions ({questions.length})
    </button>
  </div>

  <!-- Content Area -->
  {#if activeView === 'groups'}
    <Card title="Selected Groups">
      <GroupTable
        {groups}
        onUpdate={handleUpdateGroup}
        onDelete={handleDeleteGroup}
      />
    </Card>
  {:else}
    <Card title="Available Questions">
      <QuestionTable 
        {questions}
        on:update={e => handleQuestionsUpdate(e.detail)}
      />
    </Card>
  {/if}
</div>