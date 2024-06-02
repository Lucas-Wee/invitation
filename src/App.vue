<template>
  <div id="app">
      <audio autoplay loop>
      <source src="@/assets/music.mp3" type="audio/mpeg">
      Your browser does not support the audio element.
    </audio>

    <div :class="['popup', showPopup ? 'popup-active' : '']">
      <div class="popup-content">
        <p>Do you accept this invitation?</p>
        <img src="@/assets/gif1.gif" alt="Invitation Animation" />
        <button @click="closePopup">Yes</button>
        <button @click="closePopup">No</button>
      </div>
    </div>

    <div>
  <button @click="toggleMusic" class="top-left-button">Pause/Unpause Music</button>
</div>

    <video autoplay loop muted class="video-background">
      <source src="@/assets/video2.mp4" type="video/mp4">
      Your browser does not support the video tag.
    </video>
    <div class="content">
      <div class="invitation">
        <h1 class="fancy-font" @mouseenter="triggerConfetti">Niki</h1>
        <h1 class="fancy-font" @mouseenter="triggerSparkles">YOU ARE INVITED!</h1>
<div class="info-container">
          <div>
            <p><span class="white-text">{{ location }}</span></p>
            <p><span class="white-text">{{ datetime }}</span></p>
          </div>
          <img src="@/assets/gif3.gif" alt="Location and Date GIF" class="info-gif">
        </div>
      </div>
    </div>
    <CountdownTimer :date="targetDate.getTime() / 1000" />
    <footer class="designed-by">
      Designed by <a href="https://i.makeagif.com/media/2-10-2020/av6Q-M.gif" target="_blank" class="designed-by-link">Lucas</a>
    </footer>
  </div>
</template>

<script>
import CountdownTimer from './components/CountdownTimer.vue';
import party from 'party-js';

export default {
  name: 'App',
  components: {
    CountdownTimer
  },
  data() {
    return {
      showPopup: true,
      location: "Location: Tanjong Pagar MRT",
      datetime: "Date: 7 June 2024 (Fri), 7pm",
      targetDate: new Date('2024-06-07T19:00:00')
    };
  },
  methods: {

        toggleMusic() {
      if (this.audio.paused) {
        this.audio.play();
        this.musicPlaying = true;
      } else {
        this.audio.pause();
        this.musicPlaying = false;
      }
  },

    closePopup() {
      this.showPopup = false;
    },

        triggerConfetti() {
      const confettiSettings = {
        count: party.variation.range(20, 40),
        size: party.variation.range(0.6, 1.4)
      };
      party.confetti(document.querySelector('.fancy-font'), confettiSettings);
    },
    triggerSparkles() {
      const sparklesSettings = {
        count: party.variation.range(10, 60),
        speed: party.variation.range(50, 300)
      };
      party.sparkles(document.querySelector('.fancy-font'), sparklesSettings);
    }
  },
  mounted() {
    this.audio = this.$el.querySelector('audio');
    this.effectInterval = setInterval(() => {
      this.triggerConfetti();
      setTimeout(this.triggerSparkles, 2500); // Triggers 2.5 seconds after confetti
    }, 5000);
  },
  beforeUnmount() {
    clearInterval(this.effectInterval); // Clear the interval when the component is destroyed
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto+Condensed:400|Roboto:100');

#app {
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.video-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    z-index: -1;
    filter: brightness(95%);
}

.content {
    display: flex;
    width: 100%;
    justify-content: center;
    align-items: center;
}

.fancy-font {
    font-family: 'Roboto', sans-serif;
    font-size: 4em;
    color: white;
    margin: 0;
    text-align: center;
    cursor: pointer; /* Adds a pointer cursor on hover */
}

.white-text {
    font-family: 'Roboto', sans-serif;
    color: white;
    text-align: center;
    display: block;
}

.designed-by {
    position: absolute;
    bottom: 2.5%;
    width: 100%;
    text-align: center;
    color: white;
    font-family: 'Roboto', sans-serif;
}

.designed-by-link{
  color: white;
}

.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  font-family: 'Roboto', sans-serif;
  transition: opacity 1s ease-in-out, visibility 1s ease-in-out;
}

.popup-active {
  opacity: 1;
  visibility: visible;
}

/* Styling for the content box within the popup */
.popup-content {
  background: linear-gradient(to right, #ffffff, #f8f9fa);
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.2);
  text-align: center;
  width: auto;
  max-width: 50%;
  transform: scale(0.9);
  transition: transform 0.3s ease-out;
}

.popup-active .popup-content {
  transform: scale(1);
}

/* Text styling */
.popup-content p {
  font-size: 18px;
  font-weight: bold; /* Makes the text bold */
  color: #333;
  margin-bottom: 20px;
}

/* Button styling for a modern look */
button {
  margin: 10px;
  padding: 12px 25px;
  border: none;
  border-radius: 8px;
  background-color: #007BFF;
  color: white;
  font-size: 16px;
  cursor: pointer;
  font-family: 'Roboto', sans-serif;
  transition: background-color 0.2s ease-in-out;
}

button:hover {
  background-color: #0056b3;
}

.popup-content img {
  display: block; /* Ensures it does not align text weirdly */
  margin: 2px auto; /* Centers the image and provides some spacing */
  max-width: 15%; /* Ensures the image is not bigger than the popup */
  height: auto; /* Maintains aspect ratio */
}
.info-container {
  display: flex;
  align-items: center; /* Ensures vertical alignment is centered */
  justify-content: center; /* Centers content horizontally */
  gap: 30px; /* Optional: Adjust the gap between text and the GIF */
  text-align: center; /* Centers the text inside the div if it breaks into multiple lines */
}

.info-container div, .info-gif {
  flex-shrink: 0; /* Prevents flex items from shrinking */
}

.info-gif {
  max-width: 100px; /* Restrict the width of the GIF for better layout */
  height: auto; /* Maintain aspect ratio */
}

button {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}

.top-left-button {
  position: fixed; /* Fixed positioning relative to the viewport */
  top: 10px;       /* 10px from the top of the viewport */
  left: 10px;      /* 10px from the left of the viewport */
  padding: 10px 20px; /* Padding for a bigger click area and visual appeal */
  background-color: #007BFF; /* Example blue background */
  color: white; /* Text color */
  border: none; /* No border */
  border-radius: 5px; /* Rounded corners */
  cursor: pointer; /* Pointer cursor on hover */
}

.top-left-button:hover {
  background-color: #0056b3; /* Darker blue on hover for visual feedback */
}

</style>
