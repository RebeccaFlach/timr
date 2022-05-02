<template>
  <div id="app">
    <div id="main" v-bind:class="status">
      
      <Stopwatch ref="Stopwatch" id="stopwatch"/>
      <div id="times" v-if="status === 'none'">
        <p v-for="solve in history.reverse()" :key="solve">
          {{ (solve/1000).toFixed(2) }}
        </p>
      </div>
    </div>

    <div id="stats"> 
      <div id="averages">
        <div id="ao3">
          <p class="avg-label">
            ao3: 
          </p>
          <p class="avg-time">
            12.56
          </p>
        </div>
        <div id="avg">
          <p class="avg-label">
            average: 
          </p>
          <p class="avg-time">
            12.56
          </p>
        </div>
        <div id="ao12">
          <p class="avg-label">
            ao12: 
          </p>
          <p class="avg-time">
            12.56
          </p>
        </div>
      </div>
      <div id="time-table">

      </div>
      <div id="graph">
      </div>

    </div>
  </div>
</template>

<script lang="ts">
import HelloWorld from './components/HelloWorld.vue';
import Stopwatch from './components/Stopwatch.vue';
import Vue from 'vue';
import _ from 'underscore';

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
      e.preventDefault();
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
  width: 100%;
  transition-property: background-color;
  transition-duration: 150ms;
  background: #343d3f;
}
#main {
  height: 100vh;
  width: 100%;
}
#stopwatch {
  padding-top: calc(50vh - 12rem);
  font-size: 10rem;
  margin: 0 !important;
  
}
.none { 
  background: #343d3f;
}
body {
  padding: 0;
  margin: 0;
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
#times {
  font-size: 1.5rem;
}
#stats {
  border-top: 1px solid #545d5f;
  /* background-color: #020202; */
}
#averages {
  display: flex;
  justify-content: space-around;
}
</style>
