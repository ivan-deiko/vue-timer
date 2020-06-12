<template>
  <div id="app">
    <Timer 
      v-for="timer in timers"
      :key="timer.minutes"
      :timerData="timer" 
    />
  </div>
</template>

<script>
import Timer from './components/Timer';

export default {
  data() {
    return {
      dates: [
        new Date(Date.now() + (60000 * 6)),
        new Date(Date.now() + (60000 * 10)),
        new Date(Date.now() + (60000 * 15)),
      ],
    }
  },
  computed: {
    timers() {
      return this.dates.map(date => { 
        const timer = this.createTimer(this.getDiffTime(date));
        return {
          ...timer,
          endTime: date.toGMTString(),
        }
      })
    }
  },
  methods: {
    createTimer(minutes) {
      const time = {
        seconds: 59,
      };
      const remainingMinutes = minutes % 60;
      const hours = minutes / 60;

      if (remainingMinutes) {
        time.hours = Math.floor(hours);
        time.minutes = remainingMinutes - 1;
      } else {
          time.hours = hours;
          time.minutes = 59;
      }
      return time;
    },
    getDiffTime: (endTime, currentTime = new Date()) => Math.ceil((endTime.getTime() - currentTime.getTime()) / 60000)
  },
  components: {
    Timer,
  }
}
</script>

<style>
  body {
    width: 100%;
    display: flex;
    justify-content: center;
  }

</style>
