<script>
  import { onMount } from 'svelte';
  import Chart from 'chart.js/auto';

  export let easyPercentage = 40;
  export let mediumPercentage = 40;
  export let hardPercentage = 20;

  let chartCanvas;
  let chart;
  let isValid = true;

  $: {
    const total = easyPercentage + mediumPercentage + hardPercentage;
    isValid = total === 100;
  }

  onMount(() => {
    chart = new Chart(chartCanvas, {
      type: 'bar',
      data: {
        labels: ['Easy', 'Medium', 'Hard'],
        datasets: [{
          data: [easyPercentage, mediumPercentage, hardPercentage],
          backgroundColor: ['#22c55e', '#fbbf24', '#ef4444']
        }]
      },
      options: {
        plugins: { legend: { display: false } },
        scales: { y: { beginAtZero: true, max: 100 } }
      }
    });

    return () => chart.destroy();
  });

  $: if (chart) {
    chart.data.datasets[0].data = [easyPercentage, mediumPercentage, hardPercentage];
    chart.update();
  }
</script>

<div class="space-y-4">
  <div class="grid grid-cols-3 gap-4">
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Easy %*
      </label>
      <input
        type="number"
        min="0"
        max="100"
        class="w-full px-3 py-2 border rounded-md"
        bind:value={easyPercentage}
      />
    </div>
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Medium %*
      </label>
      <input
        type="number"
        min="0"
        max="100"
        class="w-full px-3 py-2 border rounded-md"
        bind:value={mediumPercentage}
      />
    </div>
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">
        Hard %*
      </label>
      <input
        type="number"
        min="0"
        max="100"
        class="w-full px-3 py-2 border rounded-md"
        bind:value={hardPercentage}
      />
    </div>
  </div>

  <div class="text-center {isValid ? 'text-green-600' : 'text-red-600'}">
    Total: {easyPercentage + mediumPercentage + hardPercentage}% 
    {isValid ? 'Valid distribution' : 'Distribution must equal 100%'}
  </div>

  <canvas bind:this={chartCanvas} class="max-h-48"></canvas>
</div>