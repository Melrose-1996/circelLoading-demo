<template>
  <div class="index" @click="run()">
    <div class="contain">
      <svg x="0px" y="0px" width="100%" height="100%" viewBox="0 0 200 200">
        <circle
          class="total"
          stroke="#fff"
          fill="none"
          stroke-width="10"
          cx="100"
          cy="100"
          r="80"
        />
        <circle
          class="path"
          fill="none"
          stroke="rgba(0, 0, 0, 0.2)"
          stroke-width="70"
          cx="100"
          cy="100"
          r="40"
        />
      </svg>
      <div class="m-pause" :style="{ display: pause ? 'flex' : 'none' }">
        <span></span>
        <span></span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'index',
  components: {},
  props: {},
  data () {
    return {
      isImmediate: false,
      number: 100,
      time: null,
      pause: false
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {
    this.circleActive()
  },
  methods: {
    run () {
      if (this.time) {
        this.pause = !this.pause
      }

      if (!this.pause) {
        this.loopRun()
      } else {
        clearTimeout(this.time)
      }
    },
    loopRun () {
      clearTimeout(this.time)
      if (this.number <= 0) {
        clearTimeout(this.time)
        this.time = null
        return
      }

      if (!this.isImmediate) {
        this.number -= 20
        this.circleActive(this.number)
        this.isImmediate = true
        this.run(this.number)
      } else {
        if (this.time && this.pause) {
          clearTimeout(this.time)
          this.isImmediate = false
          return
        }

        this.time = setTimeout(() => {
          this.number -= 20
          this.circleActive(this.number)
          this.loopRun(this.number)
        }, 1000)
      }
    },
    circleActive (val = 100) {
      const circle = document.querySelector('.path')
      const len = 2 * Math.PI * circle.getAttribute('r')

      circle.style.strokeDasharray = len
      circle.style.strokeDashoffset = len * 0.9
      circle.style.transition = 'stroke-dashoffset .3s ease-in-out'

      const rangeValue = val
      const value = len - (rangeValue / 100) * len
      circle.style.strokeDashoffset = value * -1
    }
  },
  destroyed () {
    clearTimeout(this.time)
    this.time = null
  }
}
</script>

<style lang="scss" scoped>
.index {
  position: relative;
  height: 100px;
  width: 100px;
  background-image: url('../assets/logo.png');
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;

  border-radius: 10px;
  cursor: pointer;

  .contain {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    box-sizing: border-box;

    &:hover {
      .m-pause {
        display: flex !important;
      }
    }
    .m-pause {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 15px;
      height: 15px;
      display: none;
      gap: 5px;
      & > span {
        width: 5px;
        height: 15px;
        background-color: #fff;
      }
    }

    svg {
      transform: rotate(-90deg);
    }
  }

  @keyframes run {
    to {
      stroke-dashoffset: 0;
    }
  }
}
</style>
