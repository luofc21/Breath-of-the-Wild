<template>
  <transition name="fade">
    <!-- <particles></particles> -->
      <div id="loading-page" class="loading-page" v-if="show">
         <vue-particles
          :clickEffect="true"
          :hoverEffect="true"
          :lineLinked="true"
          :lineOpacity="0.4"
          :linesDistance="150"
          :linesWidth="1"
          :moveSpeed="1"
          :particleOpacity="0.7"
          :particleSize="4"
          :particlesNumber="80"
          clickMode="push"
          color="#ccc"
          hoverMode="grab"
          linesColor="#aaa"
          shapeType="star"
          style="height:100%"
        ></vue-particles>
        <div class="loading-box">
          <p class="f-styl">LFC正在赶来啦!</p>
        </div>
      </div>
  </transition>
</template>
<script>
// import Particles from '../../components/base/particles.vue'
export default {
  props: {
    show: {
      default: true,
      type: Boolean
    }
  },
  data() {
    return {}
  },
  components: {
    // Particles,
  }
}
</script>
<style lang="scss" scoped>
.loading-page {
  z-index: 9999;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #eee;

  .loading-box {
    width: 159px;
    height: 176px;
    margin: auto;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    &:before {
      content: '';
      width: 159px;
      height: 5px;
      background: #000;
      opacity: 0.1;
      position: absolute;
      top: 186px;
      left: 0;
      border-radius: 50%;
      animation: shadow 0.5s linear infinite;
    }
    &:after {
      content: '';
      width: 159px;
      height: 176px;
      background: url('../../public/img/Luffy.png');
      animation: luffy 2s steps(4) infinite;
      border-radius: 20%;
      position: absolute;
      top: 0;
      left: 0;
      // border-radius: 3px;
    }

    p {
      position: absolute;
      bottom: -80px;
      left: 50%;
      transform: translateX(-50%);
      text-align: center;
      white-space: nowrap;
      font-size: 20px;
    }

    .f-styl {
      // font-family: 'Times New Roman', Times, serif;
      // font-style: normal;
      // font-weight: bold;
      letter-spacing: 2px;
    }
  }

  .loading-luffy {
    width: 159px;
    height: 176px;
    margin: auto;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    border-radius: 10px;
    background: url('../../public/img/Luffy.png');
    animation: luffy 0.5 step(4) infinite;
  }

  @keyframes luffy {
    from {
      background-position: 0 0;
    }
    to {
      background-position: -781px 0;
    }
  }

  @keyframes animate {
    17% {
      border-bottom-right-radius: 3px;
    }
    25% {
      transform: translateY(9px) rotate(22.5deg);
    }
    50% {
      transform: translateY(18px) scale(1, 0.9) rotate(45deg);
      border-bottom-right-radius: 40px;
    }
    75% {
      transform: translateY(9px) rotate(67.5deg);
    }
    100% {
      transform: translateY(0) rotate(90deg);
    }
  }

  @keyframes shadow {
    0%,
    100% {
      transform: scale(1, 1);
    }
    50% {
      transform: scale(1.2, 1);
    }
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>