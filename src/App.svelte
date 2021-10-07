<script>
  import Button from './Button.svelte';
  import Control from './Control.svelte';

  const kGreen = "green";
  const kRed = "red";
  const kYellow = "yellow";
  const kBlue = "blue";
  const buttons = {
    [kGreen]: {
      color: kGreen,
      sound: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound1.mp3'),
      activate: false
    },
    [kRed]: {
      color: kRed,
      sound: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound2.mp3'),
      activate: false
    },
    [kYellow]: {
      color: kYellow,
      sound: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound3.mp3'),
      activate: false
    },
    [kBlue]: {
      color: kBlue,
      sound: new Audio('https://s3.amazonaws.com/freecodecamp/simonSound4.mp3'),
      activate: false
    },
  };
  const colors = [kGreen, kRed, kYellow, kBlue];
  const list = [];

  let powerOn = false;
  let counter = 0;
  let buttonsDisabled = true;
  let userIndex = 0;
  let timeout = 0;
  let isStrict = false;

  function reset() {
    counter = 0;
    userIndex = 0;
    list.length = 0;
  }

  function clearAllInterval() {
    clearTimeout(timeout);
    timeout = -1;
  }

  function onPowerOff() {
    reset();
    isStrict = false;
    clearAllInterval();  
  }

  $: if (!powerOn) {
    onPowerOff();
  }

  function onPowerChange({ detail: on }) {
    powerOn = on;
  }

  function activateButton(color) {
    return new Promise( res => {
      buttons[color].activate = true;

      setTimeout(() => {
        buttons[color].activate = false;
        res();
      }, 750);
    });
  }

  async function secuence(newSecuence = true) {
    if (newSecuence) {
      const index = Math.floor(Math.random() * 4);
      const key = colors[index];
      list.push(key);
    }
    
    for(let color of list) {
      await activateButton(color);
    }
  }

  async function executeSecuence(newSecuence = true) {
    await secuence(newSecuence);  
    buttonsDisabled = false;
  }

  async function showTimeout() {
    if (!powerOn) {
      return onPowerOff();
    }

    const auxCounter = counter;
    buttonsDisabled = true;
    counter = -1;
    const prom = new Promise(res => {
      setTimeout(async () => {
        counter = auxCounter;
        userIndex = 0;
        await executeSecuence(false);
        res();
      }, 1000);      
    });
    await prom;
    timeout = setTimeout(showTimeout, 7500);
  }

  function actTimeout() {
    clearAllInterval();
    timeout = setTimeout(showTimeout, 7500);
  }

  async function nextColor() {
    buttonsDisabled = true;
    userIndex = 0;
    if (counter > 1) {
      const prom = new Promise(res => {
        setTimeout(async () => {
          await executeSecuence();
          res();
        }, 750);
      });
      await prom;
    } else {
      await executeSecuence();
    }

    actTimeout();
  }

  function onStart() {
    reset();
    counter++;
    nextColor();
  }

  function onButtonClick({ detail: color }) {
    const itemList = list[userIndex];
    if (itemList === color) {
      if(userIndex === list.length - 1) {
        counter++;
        if (counter < 21) {
          nextColor();
        } else {
          buttonsDisabled = true;
        }
      } else {
        userIndex++;
        actTimeout();
      }
    } else {
      if (!isStrict) {
        showTimeout();
      } else {
        buttonsDisabled = true;
        counter = -1;
        setTimeout(onStart, 2000);
      }
    }
  }

  function onButtonPreClick() {
    clearAllInterval();
  }

  function onStrictActive() {
    isStrict = !isStrict;
  }
</script>

<div class="simon">
  <div class="cont-btn cont-green">
    <Button { ...buttons.green }
      disabled={buttonsDisabled}
      on:click={onButtonClick} 
      on:pre-click={onButtonPreClick}
    />
  </div>

  <div class="cont-btn cont-red">
    <Button { ...buttons.red }
      disabled={buttonsDisabled} 
      on:click={onButtonClick}
      on:pre-click={onButtonPreClick}
    />
  </div>

  <div class="cont-btn cont-yellow">
    <Button { ...buttons.yellow }
      disabled={buttonsDisabled}
      on:click={onButtonClick} 
      on:pre-click={onButtonPreClick}
    />
  </div>

  <div class="cont-btn cont-blue">
    <Button { ...buttons.blue }
      disabled={buttonsDisabled} 
      on:click={onButtonClick}
      on:pre-click={onButtonPreClick}
    />
  </div>

  <div class="cont-center">
    <Control 
      disabled={!powerOn}
      counter={counter}
      strictActive={isStrict}

      on:power-change={onPowerChange}
      on:start={onStart}
      on:strict-active={onStrictActive}
    />
  </div>
</div>

<style>
  .simon {
    font-family: 'Gemunu Libre', sans-serif;
    border-radius: 50%;
    outline: 25px solid rgb(46, 46, 46); 
    width: 550px;
    height: 550px;
    overflow: hidden;
    position: relative;
    background-color: #000;
    box-shadow: 0px 0px 75px #000;
  }

  .cont-btn {
    position: absolute;
    width: 48%;
    height: 48%;
    overflow: inherit;
  }

  .cont-red {
    left: 53%;
  }
  
  .cont-yellow {
    top: 53%;
  }

  .cont-blue {
    top: 53%;
    left: 53%;
  }

  .cont-center {
    position: absolute;
    width: 70%;
    height: 70%;
    z-index: 1;
    top: 15%;
    left: 15%;
    border-radius: 50%;
    border: 15px solid rgb(46, 46, 46);  /*solid #bbb;*/
    background-color: rgb(219, 219, 219);
    padding: 10%;
  }

  @media screen and (max-width: 590px) {
    .simon {
      outline-width: 10px; 
      width: calc(100vw - 20px);
      height: 100vw;
      min-width: 345px;
      min-height: 345px;
    }
  }
</style>