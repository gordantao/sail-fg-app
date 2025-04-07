<script lang="ts">
  let yardLine = 35;
  let hash = "center";
  let windSpeed = 0;
  let windDirection = "none";
  let probability: number = 0.0;

  async function predictProbability() {
    // Call to your backend API
    const response = await fetch("https://your-api-url.com/predict", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ yardLine, hash, windSpeed, windDirection }),
    });

    const data = await response.json();
    probability = data.probability;
  }
</script>

<main class="p-6 max-w-md mx-auto space-y-4">
  <h1 class="text-2xl font-bold text-center">
    Field Goal Probability Predictor
  </h1>

  <div class="space-y-2">
    <label>Yard Line:</label>
    <input type="range" min="10" max="80" bind:value={yardLine} />
    <span>{yardLine} yard line</span>
  </div>

  <div class="space-y-2">
    <label>Hash Mark:</label>
    <select bind:value={hash}>
      <option value="left">Left</option>
      <option value="center">Center</option>
      <option value="right">Right</option>
    </select>
  </div>

  <div class="space-y-2">
    <label>Wind Speed (mph):</label>
    <input type="number" bind:value={windSpeed} min="0" max="50" />
  </div>

  <div class="space-y-2">
    <label>Wind Direction:</label>
    <select bind:value={windDirection}>
      <option value="none">None</option>
      <option value="headwind">Headwind</option>
      <option value="tailwind">Tailwind</option>
      <option value="crosswind">Crosswind</option>
    </select>
  </div>

  <button
    class="w-full bg-blue-600 text-white px-4 py-2 rounded"
    on:click={predictProbability}
  >
    Predict
  </button>

  {#if probability !== null}
    <div class="mt-4 text-center">
      <p class="text-xl">
        Probability: <strong>{(probability * 100).toFixed(1)}%</strong>
      </p>
    </div>
  {/if}

  <!-- Football Field Graphic -->
  <div class="relative mt-8 border border-green-800 bg-green-600 h-48">
    <div class="absolute top-0 bottom-0 w-px bg-white left-1/2"></div>
    <div
      class="absolute inset-y-0 w-12 bg-green-700/50"
      style="left: calc(100% * {yardLine} / 100);"
    ></div>
    <div class="absolute bottom-2 left-2 text-white text-xs">0</div>
    <div class="absolute bottom-2 right-2 text-white text-xs">100</div>
    <div
      class="absolute bottom-2 left-1/2 transform -translate-x-1/2 text-white text-xs"
    >
      50
    </div>
  </div>
</main>

<style>
  main {
    font-family: sans-serif;
  }
</style>
