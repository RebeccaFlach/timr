<template>
  <div id="app">
    <div id="main" v-bind:class="status">
      
      <Stopwatch ref="Stopwatch" id="stopwatch"/>
      <div id="times" v-if="status === 'none'">
        <p v-for="solve in history.slice(0,5)" :key="solve">
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
            {{ calcAvg(3) }}
          </p>
        </div>
        <div id="avg">
          <p class="avg-label">
            average: 
          </p>
          <p class="avg-time">
            {{ calcAvg() }}
          </p>
        </div>
        <div id="ao12">
          <p class="avg-label">
            ao12: 
          </p>
          <p class="avg-time">
            {{ calcAvg(12) }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Stopwatch from './components/Stopwatch.vue';
import Vue from 'vue';
import _ from 'underscore';

let waitTimeout;

const app = Vue.component('app', {
  components: {
    Stopwatch
  },
  data: () => ({
    status: 'none',
    history: [],
  }),
  methods: {
    calcAvg (numSolves?:number){
      if (!numSolves)
        numSolves = this.history.length;
      
      if (numSolves > this.history.length || this.history.length == 0)
        return "-";
      
      const solves = _(this.history).first(numSolves);
      return ((_(solves).reduce((sum, time) => sum + time, 0) / numSolves)/1000).toFixed(2);
    }, 
    handlePress (e){
      e.preventDefault();
      if (this.status === 'holding' || this.status === 'ready') 
        return;

      const timer = this.$refs.Stopwatch;
      if (this.status === 'running'){
        timer.stop();
        this.status = 'none';
        this.history.unshift(timer.time)
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
    },
    handleRelease(e){
      if (this.status === 'ready' && e.code === 'Space') {
        this.status = 'running';
        this.$refs.Stopwatch.start();
      }
      else if (this.status === 'holding') {
        this.status = 'none';
        clearTimeout(waitTimeout);
      }
    }
  },
  
  created() {
    window.addEventListener('keydown', this.handlePress);
    window.addEventListener('keyup', this.handleRelease);
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
