<!-- parent component  -->

<script>
  import ChapterSelector from '$lib/components/ChapterSelector.svelte';
  import GroupTable from '$lib/components/GroupTable.svelte';
  
  let groups = [];
  let groupCounter = 1;

  function handleGroupCreated(chapters) {
    const totalQuestions = chapters.reduce((sum, c) => sum + c.questionCount, 0);
    
    const newGroup = {
      id: groupCounter,
      description: `Group ${groupCounter}`,
      chapters: chapters.map(c => c.name),
      availableQuestions: totalQuestions,
      questionsToInsert: Math.min(10, totalQuestions)
    };
    
    groups = [...groups, newGroup];
    groupCounter++;
  }

  function handleGroupUpdate(index, field, value) {
    groups[index][field] = value;
    groups = [...groups]; // Trigger reactivity
  }

  function handleGroupDelete(index) {
    groups = groups.filter((_, i) => i !== index);
  }
</script>

<div class="space-y-8">
  <ChapterSelector onGroupCreate={handleGroupCreated} />
  
  {#if groups.length > 0}
    <GroupTable
      {groups}
      onUpdate={handleGroupUpdate}
      onDelete={handleGroupDelete}
    />
  {/if}
</div>