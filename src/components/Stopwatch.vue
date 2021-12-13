<template>
    <div>
        <p> {{(time/1000).toFixed(2) }} </p>
    </div>
</template>


<script> 
export default {
    name: 'Stopwatch',
    data () {
      return {
        timer: null,
        time: 0,
        running: false,
      }
    },
    methods: {
      start() {
        this.timer = setInterval(() => {
          this.time += 10;
        }, 10);
        this.running = true;
      },
      stop() {
        clearInterval(this.timer);
        this.running = false;
      },
      reset() {
        this.time = 0;
      }, 
      
  }
}

</script>

<style scoped>
  p {
    margin: 0;
  }


</style>
