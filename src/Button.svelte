<script>
  import createDispatch from './createDispatch';

  export let color = '';
  export let sound;
  export let activate = false;
  export let disabled = true;

  const dispatch = createDispatch();
  const clazzStatic = `btn btn-${color}`;
  let clazz = clazzStatic;

  $: activate && blink();

  function blink() {
    return new Promise( res => {
      clazz = clazzStatic + '-active';
      sound.currentIndex = 0;
      sound.play();
      setTimeout(() => {
        clazz = clazzStatic;
        res();
      }, 500);
    });
  }

  async function onClick() {
    dispatch('pre-click', color);
    await blink();
    dispatch('click', color);
  }

</script>

<button class={clazz}
  on:click={onClick}
  disabled={disabled}>
</button>

<style>
  .btn {
    width: 200%;
    height: 200%;
    border: none;
  }
</style>