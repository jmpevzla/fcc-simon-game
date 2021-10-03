<script>
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

<section class="game">
  <h1>Simon Game!</h1>

  <button disabled={mode === modes.game} 
    on:click={() => start()}>Start</button>
  <button disabled={mode === modes.init} 
    on:click={() => reset()}>Reset</button>
  <label>
    <input type="checkbox" on:change={changeStrict} checked={isStrict}> Strict Mode
  </label>
  {#if mode === modes.game}
    <p>Steps: {genArr.length}</p>
  {/if}
  {#if lastPress !== ''}
    <p>Last Press: {lastPress}</p>
  {/if}

  <div class="buttons">
    <button style={btn1Style} on:click={() => simon(1)}>Button 1</button>
    <button style={btn2Style} on:click={() => simon(2)}>Button 2</button>
    <button style={btn3Style} on:click={() => simon(3)}>Button 3</button>
    <button style={btn4Style} on:click={() => simon(4)}>Button 4</button>
  </div>
</section>