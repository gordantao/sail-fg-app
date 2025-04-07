<script lang="ts">
  let yardLine = 35;
  let windSpeed = 0;
  let probability: number = 0.0;

  async function predictProbability() {
    const response = await fetch("https://your-api-url.com/predict", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ yardLine, windSpeed }),
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
    <input type="range" min="17" max="90" bind:value={yardLine} />
    <span>{yardLine} yard line</span>
  </div>

  <div class="space-y-2">
    <label>Wind Speed (mph):</label>
    <input type="number" bind:value={windSpeed} min="0" max="50" />
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
    class="relative mt-8 border-2 border-white bg-green-700 h-56 w-full rounded shadow overflow-hidden"
  >
    <!-- Horizontal Endzones with matching stripe width and vertical text -->
    <div
      class="absolute inset-y-0 left-0 w-[10%] font-black flex flex-col items-center justify-center text-white text-xs"
      style="background-color: #56a0d3"
    >
      <span class="transform rotate-270">CAROLINA</span>
    </div>
    <div
      class="absolute inset-y-0 right-0 w-[10%] font-black flex flex-col items-center justify-center text-white text-xs"
      style="background-color: #56a0d3"
    >
      <span class="transform rotate-90">CAROLINA</span>
    </div>

    <!-- Goalpost (left side only) -->
    <div
      class="absolute top-1/2 left-[2%] w-1 h-12 bg-yellow-400 transform -translate-y-1/2"
    ></div>
    <div
      class="absolute top-[calc(50%-24px)] left-[1.25%] w-2 h-1 bg-yellow-400"
    ></div>
    <div
      class="absolute top-[calc(50%+24px)] left-[1.25%] w-2 h-1 bg-yellow-400"
    ></div>

    <!-- Alternating green stripes with updated tones -->
    {#each Array(10) as _, i}
      <div
        class="absolute top-0 bottom-0"
        style="
          left: {10 + i * 8}%;
          width: 8%;
          background-color: {i % 2 === 0 ? '#208b3a' : '#25a244'};
        "
      ></div>
    {/each}

    <!-- Midfield Logo -->
    <div
      class="absolute top-1/2 left-[50%] w-12 h-12 transform -translate-x-1/2 -translate-y-1/2 text-xs flex items-center justify-center text-white"
    >
      <img
        src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/North_Carolina_Tar_Heels_logo.svg/2560px-North_Carolina_Tar_Heels_logo.svg.png"
      />
    </div>

    <!-- Kicker Marker -->
    <div
      class="absolute w-2 h-2 bg-yellow-300 rounded-full shadow-md border border-white"
      style="
        top: 50%;
        transform: translateY(-50%);
        left: calc(({yardLine} / 100) * 80% + 10%);
      "
    ></div>
  </div>
</main>

<style>
  main {
    font-family: sans-serif;
  }
</style>
