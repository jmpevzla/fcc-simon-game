<script>
  import createDispatch from './createDispatch';

  let on = false;
  let dispatch = createDispatch();
  $: dispatch('power-change', on);
</script>

<div class="power">
  <div class="cont-subtitle">
    <h2 class="subtitle">Power</h2>
  </div>

  <label class="ctrl">
    <input type="checkbox" bind:checked={on}>
    <span class="slider"></span>
  </label>
</div>

<style>
  .ctrl {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
  }

  .ctrl input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #000;
    transition: .4s;
    border-radius: 34px;
  }

  .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: rgb(255, 255, 255);
    transition: .4s;
    border-radius: 50%;
  }

  input:checked + .slider {
    background-color: rgb(0, 100, 0); 
  }

  input:focus + .slider {
    box-shadow: 0 0 1px rgb(0, 100, 0); 
  }

  input:checked + .slider:before {
    transform: translateX(26px);
  }
</style>