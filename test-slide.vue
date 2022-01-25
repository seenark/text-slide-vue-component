<script lang="ts">
import {
  defineComponent,
  ref,
  onMounted,
} from '@nuxtjs/composition-api'
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
            10% { transform: translateX(0%); }
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

    // onUpdated(() => {
    //   span.value?.removeChild(style.value)
    //   style.value?.innerHTML = keyframe.value
    //   span.value?.appendChild(style.value)
    // })

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
      <div>
        <span ref="span">{{ text }} </span>
      </div>
    </div>
    <div class="d-flex align-center">
      <v-btn icon color="announce-title">
        <v-icon
          :class="{
            'character-green--text': $vuetify.theme.dark,
            'white--text': !$vuetify.theme.dark,
          }"
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
      //   display: flex;
      //   align-items: center;
      //   overflow-x: auto;
      overflow: hidden;
      span {
        display: block;
        white-space: nowrap;
        //   animation: name duration timing-function delay iteration-count direction fill-mode;
        //   animation: scroll-text 10s linear 5s infinite normal none;
        //   animation: scroll-text 10s linear 5s normal none;
        animation-name: scroll-text;
        animation-iteration-count: infinite;
        animation-duration: 50s;
        animation-delay: 5s;
        animation-timing-function: linear;
        animation-direction: normal;
        animation-fill-mode: both;
        animation-play-state: running;

        &:hover {
            animation-play-state: paused;
        }
      }
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

.hide-scroll {
  padding-top: 10px;
  -ms-overflow-style: none; /* Internet Explorer 10+ */
  scrollbar-width: none; /* Firefox */
  &::-webkit-scrollbar {
    width: 0; /* Remove scrollbar space */
    background: transparent; /* Optional: just make scrollbar invisible */
  }
}
</style>
