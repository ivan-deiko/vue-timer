<template>
  <div class="timer">
    <div 
      :class="`timer-container ${timerData.minutes < 5 && !timerData.hours ? 'timer-container_red' : ''}`"
    >
      <span class="timer-hour">{{hour}}-h-</span>
      <span class="timer-min">{{min}}-m-</span>
      <span class="timer-sec">{{sec}}-s</span>
    </div>
    <span class="timer-end">timer finish in {{timerData.endTime}}</span>
  </div>
</template>

<script>
  export default {
    props: {
      timerData: {
        type: Object,
        required: true,
        validator: timerData => ['seconds', 'minutes', 'hours', 'endTime'].every(key => {
          const typeProp = typeof timerData[key]
          return typeProp === 'number' || typeProp === 'string';
        })
      }
    }, 
    data() {
      return {
        timer: this.timerData,
        timerOn: true,
        timerInterval: null,
      }
    },
    computed: {
      sec() {
        return this.chageTimerItem(this.timer.seconds);
      },
      min() {
        return this.chageTimerItem(this.timer.minutes);
      },
      hour() {
        return this.chageTimerItem(this.timer.hours);
      }
    },
    watch: {
      timerOn: () => clearInterval(this.timerInterval)
    },
    methods: {
      startTimer() {
        this.timerInterval = setInterval(() => {
          if (!this.timer.seconds && !this.timer.minutes && !this.timer.hours) {
            this.timerOn = false;
          } else {
              this.timer.seconds -= 1;
              if (this.timer.seconds < 0 && this.timer.minutes) {
                // here we checked seconds 
                this.timer.minutes -= 1;
                this.timer.seconds = 59;
              } else if (!this.timer.minutes && this.timer.seconds < 0 && this.timer.hours) {
                  // here we checked hour
                  this.timer.hour -= 1;
                  this.timer.minutes = 59;
                  this.timer.seconds = 59;
              }
          }
        }, 1000);
      },
      chageTimerItem: item => String(item).padStart(2, '0'),
    },
    mounted() {
      this.startTimer();
    },
    destroyed() {
      clearInterval(this.timer)
    }
  }
</script>

<style scope>
  .timer {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 550px;
    height: 150px;
    padding: 10px 10px;
    border: 1px solid burlywood;
    box-sizing: border-box;
    font-size: 16px;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    margin-bottom: 30px;
  }
  .timer-container {
    border: 1px solid grey;
    padding: 10px;
  }
  .timer-container_red {
    border-color: red;
    color: red;
  }
  .timer-end {
    margin-left: 10px;
    font-size: 18px;
  }
</style>