<script lang="ts">
  let yardLine = 35;
  let hash = "center";
  let windSpeed = 0;
  let windDirection = "none";
  let probability: number = 0.0;

  async function predictProbability() {
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

  function hashToPercent(hash: string): string {
    switch (hash) {
      case "left":
        return "30%";
      case "right":
        return "70%";
      default:
        return "50%";
    }
  }
</script>

<main class="p-6 max-w-md mx-auto space-y-4">
  <h1 class="text-2xl font-bold text-center">
    Field Goal Probability Predictor
  </h1>

  <div class="space-y-2">
    <label>Yard Line:</label>
    <input type="range" min="15" max="85" bind:value={yardLine} />
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
  <div
    class="relative mt-8 border-2 border-white bg-green-700 h-56 rounded shadow overflow-hidden"
  >
    <!-- Horizontal Endzones -->
    <div
      class="absolute inset-y-0 left-0 w-6 bg-blue-900 flex items-center justify-center text-white text-xs"
    >
      ENDZONE
    </div>
    <div
      class="absolute inset-y-0 right-0 w-6 bg-red-900 flex items-center justify-center text-white text-xs"
    >
      ENDZONE
    </div>

    <!-- Alternating green stripes -->
    {#each Array(10) as _, i}
      <div
        class="absolute top-0 bottom-0"
        style="
          left: {6 + i * 8.8}%;
          width: 8.8%;
          background-color: {i % 2 === 0 ? '#208b3a' : '#25a244'};
        "
      ></div>
    {/each}

    <!-- Midfield Logo -->
    <div
      class="absolute top-1/2 left-1/2 w-12 h-12 bg-white/20 rounded-full transform -translate-x-1/2 -translate-y-1/2 text-xs flex items-center justify-center text-white"
    >
      LOGO
    </div>

    <!-- Kicker Marker -->
    <div
      class="absolute w-2 h-2 bg-yellow-300 rounded-full shadow-md border border-white"
      style="
        top: {hashToPercent(hash)};
        transform: translateY(-50%);
        left: calc(({yardLine} / 100) * 88% + 6%);
      "
    ></div>
  </div>
</main>

<style>
  main {
    font-family: sans-serif;
  }
</style>
