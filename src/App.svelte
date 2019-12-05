<script>
  let x = 0;
  let c = 0;
  let tail = 70;
  let hspread = 2;
  let vspread = 30;
  let speed = 0.5;
  let dotsize = 1;
  let dotopacity = 0.5;
  let gradient = 15;
  let showFullScreen = false;

  const anim = () => {
    c += speed;
    window.requestAnimationFrame(anim);
  };

  anim();

  $: x = c; // Math.abs(c % (360 + tail));
  $: y = Math.cos(x / gradient);
</script>

<style>
  .sincontainer {
    top: 0;
    left: 0;
    padding: 0;
    flex: 1 0 100px;
    border: 1px solid #999;
    margin: 4px;
  }

  .controls {
    margin: 4px;
    border: 1px solid #999;
    flex: 0 1 100px;
    display: flex;
    flex-wrap: wrap;
    color: #ccc;
    padding: 4px;
  }

  label {
    white-space: nowrap;
  }
  input {
    width: 100%;
  }

  .sindot {
    fill: #fff;
    stroke-width: 0;
  }
  .maindot {
    fill: #c30;
  }

  .swimmingpool {
    display: flex;
    flex-direction: row;
    width: 100%;
    height: 100%;
    background-color: #222;
  }

  .fullscreen {
    position: absolute;
    left: 0;
    top: 0;
    z-index: 1000;
  }
</style>

<div class="swimmingpool {showFullScreen ? 'fullscreen': ''}" >
  <svg class="sincontainer">

    {#each new Array(tail).fill() as d, i}
      <circle
        class="sindot"
        cx="{((x - i * hspread) / 3.6) % 100}%"
        cy="{Math.cos((x - i) / gradient) * vspread + 50}%"
        r={((tail / 2 - Math.abs(i - tail / 2)) / (tail / 10)) * dotsize}
        opacity={dotopacity} />
    {/each}

    <circle
      class="sindot maindot"
      cx="{(x / 3.6) % 100}%"
      cy="{y * 45 + 50}%"
      r="5" />

  </svg>

  <div class="controls">

    <div class="input-control">
      <label>Full Screen: {showFullScreen}</label>
      <input type="checkbox" bind:checked={showFullScreen} />
    </div>
    
    <div class="input-control">
      <label>tail ({tail}):</label>
      <input type="range" min="0" max="180" bind:value={tail} step="1" />
    </div>

    <div class="input-control">
      <label>speed ({speed}):</label>
      <input type="range" min="-2" max="2" bind:value={speed} step="0.01" />
    </div>

    <div class="input-control">
      <label>gradient ({gradient}):</label>
      <input
        type="range"
        min="0.01"
        max="90"
        bind:value={gradient}
        step="0.01" />
    </div>

    <div class="input-control">
      <label>vertical spread ({vspread}):</label>
      <input type="range" min="0" max="180" bind:value={vspread} step="1" />
    </div>

    <div class="input-control">
      <label>horizontal spread ({hspread}):</label>
      <input type="range" min="0" max="30" bind:value={hspread} step="0.1" />
    </div>

    <div class="input-control">
      <label>size ({dotsize}):</label>
      <input type="range" min="0.1" max="10" bind:value={dotsize} step="0.1" />
    </div>
    <div class="input-control">
      <label>opacity ({dotopacity}):</label>
      <input
        type="range"
        min="0.01"
        max="1"
        bind:value={dotopacity}
        step="0.01" />
    </div>
  </div>
</div>
