<script>
  export let count = '--';
</script>

<div class="counter">
  <div class="cont-subtitle">
    <h2 class="subtitle">Counter</h2>
  </div>

  <div class="cont-text">
    <p class="text">{count}</p>
  </div>
</div>

<style>

  .counter {
    display: flex;
    flex-direction: column;
    min-width: 6rem;
  }

  .cont-text {

    border: 2px solid #000;
    border-radius: 10px;
    text-align: center;
  }

  .text {
    font-size: 5rem;
  }

</style>