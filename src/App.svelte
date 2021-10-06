<script>
  import Button from './Button.svelte';
  import Control from './Control.svelte';

  const soundsSimon = [];
  const colorsActive = [
    'red',
    'green',
    'blue',
    'yellow'
  ];

  const modes = Object.freeze({
    'init': 'init',
    'secuence': 'secuence',
    'game': 'game',
    'end': 'end'
  });
  
  let genArr = [];
  let userIdx = 0;
  let mode = modes.init; 
  let isStrict = true;
  let lastPress = '';
  //let btnActive = -1;
  let btn1Style = '';
  let btn2Style = '';
  let btn3Style = '';
  let btn4Style = '';

  for (let i = 1; i <= 4; i++) {
    soundsSimon.push(
      new Audio(`./sounds/simonSound${i}.mp3`)
    );
  }

  function play(idx) {
    const audio = soundsSimon[idx];
    audio.currentTime = 0;  
    audio.play();
  }

  function reset() {
    userIdx = 0;
    genArr = [];
    mode = modes.init;
  }

  async function wait(time = 1000) {
    return new Promise((res) => {
      setTimeout(() => {
        res();
      }, time);
    });
  }

  function secuence() {
    genArr.forEach(async (value) => {
      const prom = new Promise(async (res) => {
        activateButton(value);
        
        setTimeout(() => {
          activateButton(-1);
        }, 2000);

        //await wait();
        res();
      });
      await prom;
    });
  }

  function activateButton (idx) {
    // if (btnActive === num - 1) {
    //   return `background-color: ${colorsActive[btnActive]}`;
    // } else {
    //   return "";
    // }

    
    btn1Style = "";
    btn2Style = "";
    btn3Style = "";
    btn4Style = "";
    
    if (idx === -1) return;
    
    play(idx);
    const base = `background-color: ${colorsActive[idx]}`;

    switch(idx) {
      case 0:
        btn1Style = base;
        break;
      case 1:
        btn2Style = base;
        break;
      case 2:
        btn3Style = base;
        break;
      case 3:
        btn4Style = base;
        break;
    }
    //console.log(btn1Style);
  }

  function generate() {
    const num = Math.floor(Math.random() * 4); 
    genArr = [...genArr, num];
    console.log(genArr);
    secuence();
  }
  
  async function simon(num) {
    if (mode === modes.game) {
      const idx = num - 1;
      activateButton(idx);
      //play(idx);
      await wait();
      console.log(idx);

      if (idx === genArr[userIdx]) {
        userIdx++;
        lastPress = num;

        if (genArr.length === 3 && userIdx === 3) {
          lastPress = 'Congratulations, You Win!';
          reset();
          return;
        }

        if (userIdx === genArr.length) {
          userIdx = 0;
          lastPress = '';
          generate();
        }
      } else {
        if (isStrict) {
          lastPress = 'Wrong Button!, You Lost!';
          reset();
          //mode = modes.game;
          //generate();
        } else {
          lastPress = 'Wrong Button, Try Again!';
          userIdx = 0;
          //secuence();
        }
      }
    }
  }

  function start() {
    if (mode === modes.init) {
      mode = modes.game;
      lastPress = '';
      generate();
    }
  }

  function changeStrict() {
    if (mode === modes.init) {
      isStrict = !isStrict;
    }
  }
</script>

<div class="simon">
  <div class="cont-btn cont-green">
    <Button color="green" />
  </div>

  <div class="cont-btn cont-red">
    <Button color="red" />
  </div>

  <div class="cont-btn cont-yellow">
    <Button color="yellow" />
  </div>

  <div class="cont-btn cont-blue">
    <Button color="blue" />
  </div>

  <div class="cont-center">
    <Control />
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