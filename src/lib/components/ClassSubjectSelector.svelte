<script>
  import Card from './Card.svelte';
  import Dropdown from './Dropdown.svelte';

  export let examClass = '';
  export let examMedium = '';
  export let examSubject = '';
  export let isValid = false;

  const classOptions = Array.from({ length: 12 }, (_, i) => (i + 1).toString());
  const mediumOptions = ['Hindi', 'English', 'Tamil'];
  const subjectOptions = ['Science', 'Maths', 'Physics'];

  let classError = false;
  let mediumError = false;
  let subjectError = false;

  $: {
    classError = !examClass;
    mediumError = !examMedium;
    subjectError = !examSubject;

    isValid = examClass !== '' && examMedium !== '' && examSubject !== '';
  }
</script>

<Card title="Class & Subject Selection">
  <div class="grid grid-cols-2 gap-4">
    <div>
      <Dropdown 
        label="Class"
        options={classOptions}
        required={true}
        bind:value={examClass}
      />
      {#if classError}
        <p class="text-sm text-red-600">Class is required</p>
      {/if}
    </div>

    <div>
      <Dropdown 
        label="Medium"
        options={mediumOptions}
        required={true}
        bind:value={examMedium}
      />
      {#if mediumError}
        <p class="text-sm text-red-600">Medium is required</p>
      {/if}
    </div>
  </div>

  <div class="mt-4">
    <Dropdown 
      label="Subject"
      options={subjectOptions}
      required={true}
      bind:value={examSubject}
    />
    {#if subjectError}
      <p class=" text-sm text-red-600">Subject is required</p>
    {/if}
  </div>
</Card>
