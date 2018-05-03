<template>
  <div>
    <div class="box b33">
      <div style="text-align: center">
        <h1 v-if='state === "stop"'>BINGO</h1>
        <div class="last-cont">
          <div v-show='state==="play" && !last'>
            <svg version="1.1" id="loader-1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
             width="200px" height="200px" viewBox="0 0 40 40" enable-background="new 0 0 40 40" xml:space="preserve">
            <path opacity="0.2" fill="#000" d="M20.201,5.169c-8.254,0-14.946,6.692-14.946,14.946c0,8.255,6.692,14.946,14.946,14.946
              s14.946-6.691,14.946-14.946C35.146,11.861,28.455,5.169,20.201,5.169z M20.201,31.749c-6.425,0-11.634-5.208-11.634-11.634
              c0-6.425,5.209-11.634,11.634-11.634c6.425,0,11.633,5.209,11.633,11.634C31.834,26.541,26.626,31.749,20.201,31.749z"/>
            <path fill="#000" d="M26.013,10.047l1.654-2.866c-2.198-1.272-4.743-2.012-7.466-2.012h0v3.312h0
              C22.32,8.481,24.301,9.057,26.013,10.047z">
              <animateTransform attributeType="xml"
                attributeName="transform"
                type="rotate"
                from="0 20 20"
                to="360 20 20"
                dur="0.5s"
                repeatCount="indefinite"/>
              </path>
            </svg>
          </div>
          <div v-show='state!=="stop" && last' class="last">{{ last }}</div>
          <div v-show='state==="pause"'>
            <i class="fa fa-pause" aria-hidden="true"></i>
          </div>
        </div>
        <div>
          <!-- <h1>
            Pizza Party & Bingo
            <small>Primeras A de Hockey Sobre Cesped - Club Gimnasia y Esgrima de Buenos Aires</small>
          </h1> -->
        </div>
      </div>
      <nav>
        <button @click.prevent='play' class="play" :disabled="state==='play'"> Comenzar </button>
        <button @click.prevent='pause' :disabled="state==='pause'"> Pausa </button>
        <button @click.prevent='stop' class="stop" :disabled="state==='stop'"> Reiniciar </button>
      </nav>
    </div>
    <div class="box b66">
      <ul>
        <li v-for='n in numbers' :key="n" :class='{ used: used.indexOf(n) !== -1 }'> {{ n }} </li>
      </ul>
    </div>
  </div>
</template>

<script>
let game;

export default {
  name: 'hello',
  data() {
    return {
      last: '',
      numbers: [],
      used: [],
      state: 'stop',
      counter: 5
    };
  },

  methods: {
    stop() {
      const stop = confirm('Estas seguro que deseas reiniciar el juego?'); //eslint-disable-line

      if (stop) {
        this.state = 'stop';
        clearInterval(game);
        this.used = [];
      }
    },

    pause() {
      this.state = 'pause';
      clearInterval(game);
    },

    play() {
      if (this.state === 'play') { return; }

      this.state = 'play';
      this.last = '';

      const getRandomInt = (min, max) => Math.floor(Math.random() * ((max - min) + 1)) + min;
      const selectNumber = () => {
        let n = getRandomInt(1, 90);

        while (this.used.indexOf(n) !== -1) { n = getRandomInt(1, 90); }

        this.last = n;
        this.used.push(n);
      };

      game = setInterval(() => {
        selectNumber();

        if (this.numbers.length === this.used.length) {
          this.state = 'done';
          clearInterval(game);
        }
      }, 10000);
    }
  },

  mounted() {
    for (let i = 1; i <= 90; i += 1) {
      this.numbers.push(i);
    }
  }
};
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}

h1 {
  font-size: 120px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 5px 5px;
}

li {
  border: 2px solid tomato;
  border-radius: 50%;
  padding: 20px;
  width: 48px;
  height: 48px;
  line-height: 50px;
  text-align: center;
  color: steelblue;
  font-size: 40px;
}

ul.used li {
  border: none;
  padding: 0;
  width: auto;
  height: auto;
  color: #3d3d3d;
  font-size: 10px;
  margin: 1px;
}

h1 small {
  display: block;
  font-size: 15px;
  font-weight: bold;
}


li.used {
  border: 2px solid #3d3d3d;
  text-decoration: line-through;
  background: #ccc;
  color: #3d3d3d;
}

.box {
  float: left;
  display: inline;
}

.b33 {
  width: 33%;
  text-align: center;
}

.b66 {
  width: 66%;
}

button {
  padding: 20px;
}

button i {
  font-size: 25px;
}

.play {
  color: green;
}

.stop {
  color: tomato;
}

.last {
  font-size: 200px;
  color: tomato;
  border-radius: 50%;
  border: 10px solid tomato;
  padding: 30px;
  height: 250px;
  width: 250px;
  display: inline-block;
}

.last-cont {
  margin-bottom: 50px;
}

svg path,
svg rect{
  fill: tomato;
}
</style>
