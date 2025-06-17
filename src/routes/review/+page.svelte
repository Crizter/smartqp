<script>
  import { selectedGroups, selectedQuestions } from '$lib/stores/questionStore';
  import Card from '$lib/components/Card.svelte';
  import ConfigurationSummary from '$lib/components/ConfigurationSummary.svelte';
  import VersioningConfiguration from '$lib/components/VersioningConfiguration.svelte';
  import QuestionDifficultyDistribution from '$lib/components/QuestionDifficultyDistribution.svelte';
  import QuestionGroupsPreview from '$lib/components/QuestionGroupsPreview.svelte';
  import ActionBar from '$lib/components/ActionBar.svelte';
  import { goto } from '$app/navigation';

  // Store subscriptions for selected data
  let groups = [];
  let questions = [];

  selectedGroups.subscribe(value => groups = value);
  selectedQuestions.subscribe(value => questions = value);
const questionGroups = [
    {
      subject: 'Mathematics',
      group: 'Algebra',
      total: 15,
      easy: 6,
      medium: 5,
      hard: 4
        },
    {
      subject: 'Mathematics',
      group: 'Geometry',
      total: 12,
      easy: 5,
      medium: 4,
      hard: 3
    },
    {
      subject: 'Physics',
      group: 'Mechanics',
      total: 18,
      easy: 7,
      medium: 6,
      hard: 5
    },
    {
      subject: 'Physics',
      group: 'Thermodynamics',
      total: 10,
      easy: 4,
      medium: 4,
      hard: 2
    },
    {
      subject: 'Chemistry',
      group: 'Organic Chemistry',
      total: 14,
      easy: 6,
      medium: 5,
      hard: 3
    }
  ];

  function handleCancel() {
    goto('/'); // Or wherever you want to navigate on cancel
  }

  function handleReset() {
    // Reset your form/store values
  }

  function handleGenerate() {
    // Generate papers logic
  }
</script>


<div class="max-w-5xl mx-auto px-4 py-8 mb-16">
  <h1 class="text-2xl font-bold mb-6">Review Question Paper</h1>
  
  <div class="space-y-8">
    <section>
      <ConfigurationSummary />
    </section>

    <section>
      <VersioningConfiguration />
    </section>

    <section>
      <QuestionDifficultyDistribution />
    </section>

    <section>
        <QuestionGroupsPreview groups = {questionGroups} />
    </section>
  </div>
</div>

<ActionBar
  totalSets={4}
  totalVersions={2}
  timePerPaper={180}
  onCancel={handleCancel}
  onReset={handleReset}
  onGenerate={handleGenerate}
/>

<!-- 

{
  subject: string,
  group: string,
  total: number,cr
  easy: number,
  medium: number,
  hard: number
} -->