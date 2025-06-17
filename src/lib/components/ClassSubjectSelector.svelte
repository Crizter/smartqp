<script>
  import Card from './Card.svelte';
  import Dropdown from './Dropdown.svelte';

  export let examClass = '';
  export let examMedium = '';
  export let examSubject = '';
  export let isValid = false;

  const classOptions = Array.from({length: 12}, (_, i) => (i + 1).toString());
  const mediumOptions = ['Hindi', 'English', 'Tamil'];
  const subjectOptions = ['Science', 'Maths', 'Physics'];

  $: isValid = examClass !== '' && examMedium !== '' && examSubject !== '';
</script>

<Card title="Class & Subject Selection">
  <div class="grid grid-cols-2 gap-4">
    <Dropdown 
      label="Class"
      options={classOptions}
      required={true}
      bind:value={examClass}
      error={examClass === '' ? 'Class is required' : ''}
    />
    <Dropdown 
      label="Medium"
      options={mediumOptions}
      required={true}
      bind:value={examMedium}
      error={examMedium === '' ? 'Medium is required' : ''}
    />
  </div>

  <Dropdown 
    label="Subject"
    options={subjectOptions}
    required={true}
    bind:value={examSubject}
    error={examSubject === '' ? 'Subject is required' : ''}
  />
</Card>