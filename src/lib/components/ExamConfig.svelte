<script>
  export let totalTime = 40;
  export let totalQuestions = 40;
  export let numberOfSets = 1;
  export let numberOfVersions = 1;
</script>

<div class="grid grid-cols-2 gap-4">
  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1">
      Total time (minutes)*
    </label>
    <input
      type="number"
      min="30"
      max="180"
      required
      class="w-full px-3 py-2 border rounded-md"
      bind:value={totalTime}
    />
  </div>

  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1">
      Total questions*
    </label>
    <input
      type="number"
      required
      class="w-full px-3 py-2 border rounded-md"
      bind:value={totalQuestions}
    />
  </div>

  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1">
      Number of sets*
    </label>
    <input
      type="number"
      min="1"
      max="5"
      required
      class="w-full px-3 py-2 border rounded-md"
      bind:value={numberOfSets}
    />
  </div>

  <div>
    <label class="block text-sm font-medium text-gray-700 mb-1">
      Number of versions*
    </label>
    <input
      type="number"
      min="1"
      max="10"
      required
      class="w-full px-3 py-2 border rounded-md"
      bind:value={numberOfVersions}
    />
  </div>
</div>