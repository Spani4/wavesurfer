<template>
  <div  id="app">
    <div class="container">
      <h1 class="mb-5">Wavesurfer testing</h1>
      <div class="p-4 border border-primary rounded">
        <div v-if="wavesurfer" class="row py-3">
          <div class="col-auto">
            &times; {{ rate }}
          </div>
          <div class="col-auto flex-grow-1"></div>
          <div class="col-auto">
            {{ progress }}
          </div>
          <div class="col-auto">
            {{ duration }}
          </div>
        </div>
        <div ref="wavesurfer" class="mb-5"></div>
        <div class="bg-light py-3">
          <button class="btn btn-primary me-3" @click="stopPlay">Stop</button>
          <button class="btn btn-primary me-3" @click="startPlay">Play</button>
          <button class="btn btn-primary me-3" @click="playPause">Play/Pause</button>
          <div class="w-100 py-1"></div>
          <button class="btn btn-primary me-3" @click="slowDown">Slow down</button>
          <button class="btn btn-primary me-3" @click="speedUp">Speed Up</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import WaveSurfer from 'wavesurfer.js'

export default {
  name: 'App',
  data() {
    return {
      wavesurfer: null,
      progress: '00:00:00'
    }
  },

  mounted() {
    this.wavesurfer = WaveSurfer.create({
      container: this.$refs.wavesurfer,
      waveColor: 'blue',
      barWidth: 1,
      barHeight: 1,
      barGap: null,
      progressColor: 'darkBlue'
    })

    this.wavesurfer.load('/files/demo.mp3')

    this.wavesurfer.on('audioprocess', (event) => {
      this.progress = this.secondsToHms(event)
    })
  },

  computed: {
    duration () {
      return this.secondsToHms(this.wavesurfer.getDuration())
    },

    rate () {
      return this.wavesurfer.getPlaybackRate()
    }
  },

  methods: {
    secondsToHms (seconds) {
      const d = Number(seconds);
      const h = Math.floor(d / 3600);
      const m = Math.floor(d % 3600 / 60);
      const s = Math.floor(d % 3600 % 60);

      return ('0' + h).slice(-2) + ":" + ('0' + m).slice(-2) + ":" + ('0' + s).slice(-2);
    },
    startPlay () {
      this.wavesurfer.play()
    },
    stopPlay () {
      this.wavesurfer.stop()
    },
    playPause () {
      this.wavesurfer.playPause()
    },
    slowDown () {
      this.wavesurfer.setPlaybackRate(Math.floor((this.wavesurfer.getPlaybackRate() - 0.1) * 10) / 10)
    },
    speedUp () {
      this.wavesurfer.setPlaybackRate(Math.floor((this.wavesurfer.getPlaybackRate() + 0.1) * 10) / 10)
    },
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


</style>
