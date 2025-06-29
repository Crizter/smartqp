<script>
  import { onMount } from 'svelte';
  import Chart from 'chart.js/auto';
  import Card from './Card.svelte';

  // Add new props while keeping existing ones
  export let easy = 40;
  export let medium = 40;
  export let hard = 20;
  export let isValid = true;
  export let isReviewPageEnabled = false;
  export let disabled = false; // Add this line

  // Rename existing percentage variables to match the chart functionality
  $: easyPercentage = easy;
  $: mediumPercentage = medium;
  $: hardPercentage = hard;

  let chartCanvas;
  let chart;

  let easyValid = true;
  let mediumValid = true;
  let totalValid = true;
  let hardValid = true;

  $: {
    const total = easyPercentage + mediumPercentage + hardPercentage;
    isValid = total === 100;
  }

  onMount(() => {
    chart = new Chart(chartCanvas, {
      type: 'bar',
      data: {
        labels: ['Distribution'],
        datasets: [
          {
            label: 'Easy',
            data: [easyPercentage],
            backgroundColor: '#22c55e',
            barPercentage: 0.8
          },
          {
            label: 'Medium',
            data: [mediumPercentage],
            backgroundColor: '#fbbf24',
            barPercentage: 0.8
          },
          {
            label: 'Hard',
            data: [hardPercentage],
            backgroundColor: '#ef4444',
            barPercentage: 0.8
          }
        ]
      },
      options: {
        indexAxis: 'y',
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: { display: false }
        },
        scales: {
          x: {
            stacked: true,
            display: false,
            max: 100
          },
          y: {
            stacked: true,
            display: false
          }
        }
      }
    });

    return () => chart.destroy();
  });

  $: if (chart) {
    chart.data.datasets[0].data = [easyPercentage];
    chart.data.datasets[1].data = [mediumPercentage];
    chart.data.datasets[2].data = [hardPercentage];
    chart.update();
  }
</script>

<div class="space-y-4">
  <div class="grid grid-cols-3 gap-4">
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Easy %*
      </label>
      {#if isReviewPageEnabled}
        <div class="flex items-center justify-between w-full px-4 py-2.5 bg-green-50 border border-green-100 rounded-lg">
          <span class="text-sm font-medium text-gray-900">{easy}%</span>
          <span class="h-2 w-2 rounded-full bg-green-400"></span>
        </div>
      {:else}
        <input 
          type="number"
          min="0"
          max="100"
          bind:value={easy}
          class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 {disabled ? 'bg-gray-50 cursor-not-allowed' : ''}"
          {disabled}
        />
      {/if}
    </div>

    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Medium %*
      </label>
      {#if isReviewPageEnabled}
        <div class="flex items-center justify-between w-full px-4 py-2.5 bg-yellow-50 border border-yellow-100 rounded-lg">
          <span class="text-sm font-medium text-gray-900">{medium}%</span>
          <span class="h-2 w-2 rounded-full bg-yellow-400"></span>
        </div>
      {:else}
        <input 
          type="number"
          min="0"
          max="100"
          bind:value={medium}
          class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 {disabled ? 'bg-gray-50 cursor-not-allowed' : ''}"
          {disabled}
        />
      {/if}
    </div>

    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Hard %*
      </label>
      {#if isReviewPageEnabled}
        <div class="flex items-center justify-between w-full px-4 py-2.5 bg-red-50 border border-red-100 rounded-lg">
          <span class="text-sm font-medium text-gray-900">{hard}%</span>
          <span class="h-2 w-2 rounded-full bg-red-400"></span>
        </div>
      {:else}
        <input 
          type="number"
          min="0"
          max="100"
          bind:value={hard}
          class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 {disabled ? 'bg-gray-50 cursor-not-allowed' : ''}"
          {disabled}
        />
      {/if}
    </div>
  </div>

  <div class="text-center {isValid ? 'text-green-600' : 'text-red-600'}">
    Total: {easyPercentage + mediumPercentage + hardPercentage}% 
    {isValid ? 'Valid distribution' : 'Distribution must equal 100%'}
  </div>

  <div>
    <p class="font-medium font-sans text-gray-700">Distribution preview</p>
    <canvas bind:this={chartCanvas} class="max-h-12"></canvas>
  </div>
</div>