<template>
    <div>
        <p> {{formatTime(time) }} </p>
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
      formatTime(time) {
        const sec = Math.floor(time/1000);
        const ms = time % 1000 + "";
        console.log(ms)
        return sec + ":" + (ms[0] || 0 ) + (ms[1] || 0);
      }
  }
}

</script>

<style scoped>

</style>
