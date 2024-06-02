<template>
  <div class="content">
    <div class="block">
      <p class="digit">{{ twoDigitDays }}</p>
      <p class="text">Days</p>
    </div>
    <div class="block">
      <p class="digit">{{ twoDigitHours }}</p>
      <p class="text">Hours</p>
    </div>
    <div class="block">
      <p class="digit">{{ twoDigitMinutes }}</p>
      <p class="text">Minutes</p>
    </div>
    <div class="block">
      <p class="digit">{{ twoDigitSeconds }}</p>
      <p class="text">Seconds</p>
    </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            now: Math.floor(new Date().getTime() / 1000),
        };
    },
    props: {
        date: {
            type: Number,
            default: () => Math.floor(new Date().getTime() / 1000) + 86400, // Default to 24 hours from now
        },
    },
    created() {
        setInterval(() => {
            this.now = Math.floor(new Date().getTime() / 1000);
        }, 1000);
    },
    computed: {
        seconds() {
            const secs = (this.date - this.now) % 60;
            return secs < 0 ? 0 : secs;
        },
        minutes() {
            const mins = Math.floor((this.date - this.now) / 60) % 60;
            return mins < 0 ? 0 : mins;
        },
        hours() {
            const hrs = Math.floor((this.date - this.now) / 3600) % 24;
            return hrs < 0 ? 0 : hrs;
        },
        days() {
            const days = Math.floor((this.date - this.now) / 86400);
            return days < 0 ? 0 : days;
        },
        twoDigitDays() {
            return this.days.toString().padStart(2, '0');
        },
        twoDigitHours() {
            return this.hours.toString().padStart(2, '0');
        },
        twoDigitMinutes() {
            return this.minutes.toString().padStart(2, '0');
        },
        twoDigitSeconds() {
            return this.seconds.toString().padStart(2, '0');
        },
    },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto+Condensed:400|Roboto:100');

.content {
    display: flex;
    width: 100%; /* Ensure the countdown takes full width */
    justify-content: space-around; /* Distribute space evenly around items */
    align-items: center; /* Align items vertically in the center */
    font-family: 'Roboto', sans-serif;
}

.block {
    flex-grow: 1; /* Each block will grow evenly */
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
}

.digit {
    font-size: 4vw; /* Responsive font size based on viewport width */
    color: #ecf0f1;
    margin-bottom: 1px; /* Reduced margin-bottom to bring the text closer */
}

.text {
    font-size: 1vw;
    color: #69F831;
}
</style>