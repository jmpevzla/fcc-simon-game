<script>
  import createDispatch from './createDispatch';
  export let disabled = true;

  const dispatch = createDispatch();

  function onClick() {
    dispatch('start');
  }
</script>

<div class="start">
  <div class="cont-subtitle">
    <h2 class="subtitle">Start</h2>
  </div>
  <div class="cont-control">
    <button class="btn-ctrl btn-start"
      disabled={disabled}
      on:click={onClick}></button>
  </div>
</div>

<style>
  .start {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  .cont-control {
    height: inherit;
  }

  .btn-start {
    background-color: rgb(216, 0, 0);
    min-width: 3.8rem;
    min-height: 3.8rem;
  }

</style>