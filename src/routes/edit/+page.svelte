<script>
  import PaperTable from '$lib/components/PaperTable.svelte';
  import { goto } from '$app/navigation';

  // Props to receive papers data
  export let data;

  // Sample data structure (replace with your actual data)
  let papers = [
    {
      questionPaperId: 'QP001',
      eventName: 'Mid Term Exam',
      subjectName: 'Mathematics',
      standard: '10th',
      medium: 'English',
      createdAt: '2023-06-23',
      status: 'Draft',
      examMode: 'Online',
      totalTime: 180,
      totalQuestions: 50,
      numberOfSets: 2,
      numberOfVersions: 1,
      groups: [],
      difficultyDistribution: {
        easy: 30,
        medium: 50,
        hard: 20
      }
    }
    // ...more papers
  ];

  function handleEdit(event) {
    const { paperId } = event.detail;
    goto(`/edit/${paperId}`);
  }
</script>

<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
  <PaperTable 
    {papers}
    on:edit={handleEdit}
  />
</div>