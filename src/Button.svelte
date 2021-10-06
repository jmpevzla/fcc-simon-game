<script>
  export let color = "green";

  const clazzStatic = `btn btn-${color}`;
  let clazz = clazzStatic;

  function activate() {
    clazz = clazzStatic + '-active';
    setTimeout(() => {
      clazz = clazzStatic;
    }, 500);
  }

  function onClick() {
    console.log(color, Math.random())
    activate();
  }

</script>

<div class={clazz}
  on:click={onClick}>
</div>

<style>
  .btn {
    width: 200%;
    height: 200%;
  }
</style>