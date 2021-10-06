<script>
  import Power from './Power.svelte';
  import Start from './Start.svelte';
  import Counter from './Counter.svelte';
  import Strict from './Strict.svelte';
</script>

<section class="container">
  <header class="cont-title">
    <h1 class="title">Simon <sup>&reg;</sup></h1>
  </header>

  <div class="board">
    <Power />
    <Start />
  </div>

  <div class="board">
    <Counter />
    <Strict />
  </div>
</section>

<style>
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    justify-content: space-between;
    align-items: center;
    user-select: none;
  }

  .title {
    font-size: 5rem;
    font-weight: bold;
  }

  .title sup {
    font-size: 2.5rem;
    margin-left: -0.5rem;
  }

  .board {
    display: flex;
    justify-content: space-around;
    width: 100%;
  }
</style>