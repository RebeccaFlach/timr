<template>
  <div id="app" v-bind:class="{onHold: buttonHeld, running: running, ready: ready, app: true}">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Stopwatch ref="Stopwatch"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import Stopwatch from './components/Stopwatch.vue';
import Vue from 'vue';


const app = Vue.component('app', {
  // name: 'App',
  components: {
    HelloWorld,
    Stopwatch
  },
  data: () => ({
    buttonHeld: false,
    ready: false,
    running: false,
  }),
  
  created() {
    window.addEventListener('keydown', (e) => {
      if (this.buttonHeld) {
        return;
      }

      const timer = this.$refs.Stopwatch;
      if (timer.running){
        timer.stop();
        this.running = false;
      }
      else {
        if (e.code === 'Space') {
          timer.reset();
          this.buttonHeld = true;
          setTimeout(() => {
            this.ready = true;
          }, 500);
        }
      }
      
    });
    window.addEventListener('keyup', (e) => {
      if (this.buttonHeld && e.code === 'Space') {
        this.buttonHeld = false;
        this.ready = false;
        this.running = true;
        this.$refs.Stopwatch.start();
      }

    })            
  },
})



export default app;

</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f9fbfc;
  font-size: 10rem;
  background: #2d3436;
  margin: 0;
  padding: 0;
  height: 100%;
  position: absolute;
  top: 0;
  width: 100%;
  transition-property: background-color;
  transition-duration: 200ms;

}
body {
  padding: 0;
  margin: 0;
  height: 100%;
  top: 0;
}
html {
  height: 100%;
}
.onHold {
  background: #4d0b0b;
}
.running {
  background: #121212;
}
.ready {
  background: #1d633a;
}
</style>
