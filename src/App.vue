<template>
  <div id="app" v-bind:class="status">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Stopwatch ref="Stopwatch" id="stopwatch"/>
    <div id="example-1">
      <p v-for="solve in history" :key="solve">
        {{ (solve/1000).toFixed(2) }}
      </p>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import Stopwatch from './components/Stopwatch.vue';
import Vue from 'vue';
import _ from 'underscore'

let waitTimeout;

const app = Vue.component('app', {
  components: {
    HelloWorld,
    Stopwatch
  },
  data: () => ({
    status: 'none',
    history: [],
  }),
  
  created() {
    window.addEventListener('keydown', (e) => {
      if (this.status === 'holding' || this.status === 'ready') 
        return;
      

      const timer = this.$refs.Stopwatch;
      if (this.status === 'running'){
        timer.stop();
        this.status = 'none';
        this.history.push(timer.time);
        console.log(this.history)
      }
      else {
        if (e.code === 'Space') {
          timer.reset();
          this.status = 'holding';
          waitTimeout = setTimeout(() => {
            this.status = 'ready';
          }, 700);
        }
      }
      
    });
    window.addEventListener('keyup', (e) => {
      if (this.status === 'ready' && e.code === 'Space') {
        this.status = 'running';
        this.$refs.Stopwatch.start();
      }
      else if (this.status === 'holding') {
        this.status = 'none';
        clearTimeout(waitTimeout);
      }

    })            
  },
})



export default app;

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f9fbfc;
  
  margin: 0;
  padding: 0;
  height: 100%;
  position: absolute;
  top: 0;
  width: 100%;
  transition-property: background-color;
  transition-duration: 150ms;
}
#stopwatch {
  font-size: 10rem;
}
.none { 
  background: #343d3f;
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
.holding {
  background: #4d0b0b;
}
.running {
  background: #020202;
}
.ready {
  background: #134b2a;
}
</style>
