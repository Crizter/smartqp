<script>
  export let currentView = 'config';

  const steps = [
    { 
      id: 'config', 
      label: 'Create Paper',
      icon: '📝',
      description: 'Set up exam parameters'
    },
    { 
      id: 'review', 
      label: 'Review',
      icon: '👀',
      description: 'Review configuration'
    },
    { 
      id: 'generate', 
      label: 'Generate Paper',
      icon: '⚡',
      description: 'Create exam papers'
    },
    { 
      id: 'view', 
      label: 'View Paper',
      icon: '📋',
      description: 'Preview generated papers'
    }
  ];

  $: getStepStatus = (stepId) => {
    const stepIndex = steps.findIndex(step => step.id === stepId);
    const currentIndex = steps.findIndex(step => step.id === currentView);
    
    if (stepIndex < currentIndex) return 'completed';
    if (stepIndex === currentIndex) return 'active';
    return 'pending';
  };
</script>

<div class="py-8 px-6">
  <div class="flex items-center relative">
    {#each steps as step, index}
      <div class="flex items-center flex-1 {index === steps.length - 1 ? 'flex-initial' : ''}">
        <!-- Step circle -->
        <div class="relative flex flex-col items-center">
          <div class="flex items-center justify-center w-12 h-12 rounded-full border-2 
            transition-all duration-300 ease-in-out transform hover:scale-110 z-10
            {getStepStatus(step.id) === 'completed' ? 'bg-primary border-primary shadow-lg' :
              getStepStatus(step.id) === 'active' ? 'bg-white border-primary shadow-md' :
              'bg-white border-gray-300'}"
          >
            {#if getStepStatus(step.id) === 'completed'}
              <svg class="w-6 h-6 text-white" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/>
              </svg>
            {:else}
              <span class="text-xl">{step.icon}</span>
            {/if}
          </div>

          <!-- Label and Description -->
          <div class="mt-3 text-center">
            <span class="block font-semibold {
              getStepStatus(step.id) === 'completed' ? 'text-primary' :
              getStepStatus(step.id) === 'active' ? 'text-primary' :
              'text-gray-500'
            } transition-colors duration-300">{step.label}</span>
            <span class="text-xs text-gray-500 mt-1 block max-w-[120px]">
              {step.description}
            </span>
          </div>
        </div>

        <!-- Connector Line -->
        {#if index !== steps.length - 1}
          <div class="flex-1 h-[2px] mx-4 transition-all duration-500 ease-in-out
            {getStepStatus(step.id) === 'completed' ? 'bg-primary' : 'bg-gray-300'}"
          ></div>
        {/if}
      </div>
    {/each}
  </div>
</div>

<style>
  .shadow-md, .shadow-lg {
    transition: box-shadow 0.3s ease-in-out;
  }
</style>