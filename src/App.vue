<template>
  <div id="app" v-bind:class="status">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Stopwatch ref="Stopwatch"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import Stopwatch from './components/Stopwatch.vue';
import Vue from 'vue';

let waitTimeout;

const app = Vue.component('app', {
  components: {
    HelloWorld,
    Stopwatch
  },
  data: () => ({
    status: 'none'
  }),
  
  created() {
    window.addEventListener('keydown', (e) => {
      if (this.status === 'holding' || this.status === 'ready' || this.status === 'wait') {
        return;
      }
      console.log('keydown')

      const timer = this.$refs.Stopwatch;
      if (this.status === 'running'){
        timer.stop();
        this.status = 'none';
      }
      else {
        if (e.code === 'Space') {
          timer.reset();
          this.status = 'holding';
          waitTimeout = setTimeout(() => {
            this.status = 'ready';
          }, 500);
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
  font-size: 10rem;
  margin: 0;
  padding: 0;
  height: 100%;
  position: absolute;
  top: 0;
  width: 100%;
  transition-property: background-color;
  transition-duration: 200ms;
}
.none { 
  background: #2d3436;
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
  background: #121212;
}
.ready {
  background: #1d633a;
}
</style>
