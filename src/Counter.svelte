<script>
  export let count = 0;
  export let disabled = true;

  const end = 21;
  let textBaseClazz = "text";
  let textClazz = textBaseClazz;

  $: counter = () => {
    if (count === -1) return "!!";
    if (count === end) return "W!";

    const init = "--";
    const cs = count >= 10 ? count.toString() : '0' + count.toString();
    const value = count === 0 ? init : cs;
    return disabled ? '' : value;
  }

  $: if ((count === 0 || count === -1 || count === end) 
    && !disabled 
    && textClazz === textBaseClazz) {
    
    textClazz += " blink";
  } else {
    textClazz = textBaseClazz;
  }
    
</script>

<div class="counter">
  <div class="cont-subtitle">
    <h2 class="subtitle">Counter</h2>
  </div>

  <div class="cont-text">
    <p class={textClazz}>{counter()}</p>
  </div>
</div>

<style>

  .counter {
    display: flex;
    flex-direction: column;
    min-width: 6rem;
  }

  .cont-text {
    min-height: 5.8rem;
    border: 2px solid #000;
    border-radius: 10px;
    text-align: center;
  }

  .text {
    font-size: 5rem;
  }

  .blink {
    animation: counter .75s infinite ease-out; 
  }

  @keyframes counter {
    0% {
      opacity: 1;
    }
    100% {
      opacity: 0;
    }
  }
</style>