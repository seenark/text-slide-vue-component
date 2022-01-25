<script lang="ts">
import { defineComponent, ref, onMounted } from '@nuxtjs/composition-api'
export default defineComponent({
  setup() {
    const announce = ref(true)
    const text = ref('')
    const span = ref<HTMLElement>()
    const div = ref<HTMLElement>()
    const style = ref<HTMLStyleElement>()

    const keyframe = () => {
      if (!span.value) return ''
      const tx = span.value.scrollWidth - span.value.clientWidth
      return `
        @keyframes scroll-text {
            5% { transform: translateX(0%); }
            80% { transform: translateX(-${tx}px); }
            90% { transform: translateX(-${tx}px); }
            100% { transform: translateX(0%); }
        }
        `
    }

    function setAnimation() {
      if (!span.value) return
      if (span.value.getElementsByTagName('style') && style.value) {
        span.value?.removeChild(style.value)
      }

      style.value = document.createElement('style')
      style.value.innerHTML = keyframe()
      span.value?.appendChild(style.value)
    }

    function watchResize() {
      setAnimation()
    }

    onMounted(() => {
      new ResizeObserver(watchResize).observe(span.value!)

      text.value = `Binance will list Anchor Protocol (ANC) and will open trading for ANC/BTC, ANC/BUSD and ANC/USDT trading pairs at 2022-01-25 11:00 (UTC).
                    Users can now start depositing ANC in preparation for trading
                    Withdrawals for ANC will open at 2022-01-26 11:00 (UTC)
                    Note: The withdrawal open time is an estimated time for users’ reference. Users can view the actual status on the withdrawal page------------`
      setAnimation()
    })

    return {
      announce,
      text,
      span,
      div,
    }
  },
})
</script>

<template>
  <div v-if="announce" class="annc-box announce-bg font-maven-pro">
    <div class="announce-title--text d-flex align-center">
      <img
        v-if="!$vuetify.theme.dark"
        width="24"
        height="24"
        src="~/assets/loudspeaker.svg"
        alt=""
      />
      <img
        v-else
        width="24"
        height="24"
        src="~/assets/loudspeaker-white.svg"
        alt=""
      />
      <span
        style="text-transform: uppercase; font-size: 16px; font-weight: 700"
        class="ml-2"
        >Announcement</span
      >
    </div>
    <div ref="div" class="text-box white--text">
      <div v-if="!$vuetify.theme.dark" style="background: linear-gradient(270deg, rgba(0, 0, 0, 0) 0%, #253832 100%);" class="before"></div>
      <div v-if="$vuetify.theme.dark" style="background: linear-gradient(270deg, rgba(0, 0, 0, 0) 0%, #32A17F 100%);" class="before"></div>
      <div>
        <span ref="span">{{ text }} </span>
      </div>
      <div v-if="!$vuetify.theme.dark" style="background: linear-gradient(90deg, rgba(0, 0, 0, 0) 0%, #253832 100%);" class="after"></div>
      <div v-if="$vuetify.theme.dark" style="background: linear-gradient(90deg, rgba(0, 0, 0, 0) 0%, #32A17F  100%);" class="after"></div>
    </div>
    <div class="d-flex align-center">
      <v-btn icon color="announce-title" @click="announce = false">
        <v-icon
          
          color="white"
          >mdi-close</v-icon
        >
      </v-btn>
    </div>
  </div>
</template>

<style lang="scss">
.annc-box {
  padding: 0 16px;
  display: grid;
  grid-template-columns: auto 5fr 24px;
  gap: 8px;
  max-height: 44px;

  .text-box {
    overflow: hidden;
    display: flex;
    align-items: center;
    div {
      overflow: hidden;
      span {
        display: flex;
        align-items: center;
        height: 44px;
        white-space: nowrap;
        padding: 0 16px;

        animation-name: scroll-text;
        animation-iteration-count: infinite;
        animation-duration: 50s;
        animation-delay: 0s;
        animation-timing-function: linear;
        animation-direction: normal;
        animation-fill-mode: both;
        animation-play-state: running;

        &:hover {
          animation-play-state: paused;
        }
      }
    }
    .before {
        z-index: 1;
      position: absolute;
      left: 180px;
      top: 0;
      height: 44px;
      width: 44px;
    //   background: linear-gradient(270deg, rgba(37, 56, 50, 0) 0%, #253832 100%);
    }
    .after {
        z-index: 1;
      position: absolute;
      right: 44px;
      top: 0;
      height: 44px;
      width: 44px;
    //   background: linear-gradient(90deg, rgba(37, 56, 50, 0) 0%, #253832 100%);
    }

    // @keyframes scroll-text {
    //     0% {
    //         transform: translateX(0%);
    //     }

    //     90% {
    //         transform: translateX(-2079px);
    //     }

    //     100% {
    //         transform: translateX(0%);
    //     }
    // }
  }
}
</style>
