<script>

  let active = false;

  const baseLed = "led";
  $: clazzLed = baseLed + (active ? ' led-active' : '');

  function onClick() {
    active = !active;
  }
</script>

<div class="strict">
  <div class="cont-subtitle">
    <h2 class="subtitle">Strict</h2>
  </div>

  <div class="cont-control">
    <div class={clazzLed}></div>
    <div class="cont-btn">
      <button class="btn-ctrl btn-strict" 
        on:click={onClick}></button>
    </div>
  </div>
</div>

<style>
  .strict {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .cont-control {
    height: inherit;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }

  .cont-btn {
    height: calc(100% - 2.5rem);
  }

  .btn-strict {
    background-color: rgb(0, 100, 0);
  }

  .led {
    width: 1.5rem;
    height: 1.5rem;
    background-color: hsl(0, 100%, 15%);
    border-radius: 50%;
    margin: 0 auto .5rem auto;
    border: 1px solid #000;
    transition: all .5s ease;
  }

  .led-active {
    background-color: hsl(0, 100%, 45%);
  }
</style>