<script>
  import { selectedGroups } from '$lib/stores/questionStore';
  import TabButton from './TabButton.svelte';
  import ActionButton from './ActionButton.svelte';
  import SummaryTable from './SummaryTable.svelte';
  import QuestionsList from './QuestionsList.svelte';
  
  export let visible = false;
  export let onGroupAdded = () => {};
  
  let groups = [];
  let activeTab = 'chapters';

  const deleteIcon = `<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />`;
  const filterIcon = `<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 4a1 1 0 011-1h16a1 1 0 011 1v2.586a1 1 0 01-.293.707l-6.414 6.414a1 1 0 00-.293.707V17l-4 4v-6.586a1 1 0 00-.293-.707L3.293 7.293A1 1 0 013 6.586V4z" />`;

  export function handleAddGroup(group) {
    const newGroup = {
      number: groups.length + 1,
      description: group.description || '',
      availableQuestions: group.questions || 0,
      questionsToInsert: group.questionsToInsert || 0
    };
    
    groups = [...groups, newGroup];
    onGroupAdded(newGroup);
  }
  
  function updateGroup(index, field, value) {
    groups = groups.map((group, i) => {
      if (i === index) {
        return { ...group, [field]: value };
      }
      return group;
    });
    selectedGroups.set(groups);
  }

  function deleteGroup(index) {
    groups = groups.filter((_, i) => i !== index);
    selectedGroups.set(groups);
  }
</script>

{#if visible}
  <div class="bg-white rounded-lg shadow mt-6">
    <div class="p-4">
      <h2 class="text-2xl font-bold mb-6">Summary section</h2>
      
      <div class="inline-flex rounded-lg border border-gray-200 mb-6">
        <TabButton 
          label="Chapters"
          active={activeTab === 'chapters'}
          position="left"
          on:click={() => activeTab = 'chapters'}
        />
        <TabButton 
          label="Questions selected"
          active={activeTab === 'questions'}
          position="right"
          on:click={() => activeTab = 'questions'}
        />
      </div>

      {#if activeTab === 'chapters'}
        <div class="space-y-4">
          <div class="flex justify-between items-center border-b pb-4">
            <h3 class="text-lg font-medium">Subject summary</h3>
            <div class="flex items-center gap-4">
              <ActionButton 
                label="Delete"
                icon={deleteIcon}
                variant="danger"
              />
              <ActionButton 
                label="Filters"
                icon={filterIcon}
              />
            </div>
          </div>

          <SummaryTable 
            {groups}
            onUpdateGroup={updateGroup}
            onDeleteGroup={deleteGroup}
          />
        </div>
      {:else}
        <QuestionsList />
      {/if}
    </div>
  </div>
{/if}