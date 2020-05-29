<template>
  <div class="home" :class="{ fullscreen: isFullscreen }">
    <div
      v-for="(video, index) in options"
      :key="index"
      :id="index"
      :class="[{ fullscreen: video.fullscreen }, 'video-wrapper']"
      v-focus
      data-left="AUTOFOCUS"
      data-right="AUTOFOCUS"
      data-up="AUTOFOCUS"
      data-down="AUTOFOCUS"
      @click="play(index)"
      :tabindex="index"
      :style="{ backgroundImage: 'url(' + video.poster + ')' }"
    >
      <LivePlayer
        class="video"
        v-if="isFullscreen"
        :class="{ fullscreen: isFullscreen }"
        :options="video"
        ref="options"
      ></LivePlayer>
    </div>
  </div>
</template>

<script>
/*eslint-disable */
import { navigationService } from "vue-spatialnavigation";
import LivePlayer from "@/components/LivePlayer";
export default {
  name: "Home",
  components: {
    LivePlayer,
  },
  data() {
    return {
      options: [
        {
          src: "https://dev-live.livetvstream.co.uk/LS-63503-4/index.m3u8",
          poster: "https://d2czrg17gnlnc9.cloudfront.net/Live.png",
          //fullscreen: false,
        },
        {
          src: "https://d2rwmwucnr0d10.cloudfront.net/live.m3u8",
          poster:
            "https://cdn6.aptoide.com/imgs/e/c/d/ecd9e30cdf9aab20a3540ebd62e700ee.png",
          //fullscreen: false,
        },
        {
          src: "https://dev-live.livetvstream.co.uk/LS-63503-4/index.m3u8",
          poster:
            "https://c.files.bbci.co.uk/117BF/production/_108551617_mediaitem108551616.jpg",
          //fullscreen: false,
        },
      ],
      isFullscreen: false,
      selected: navigationService.getFocusElementInFocus(),
    };
  },
  methods: {
    async play(i) {
      await this.setFullscreen(i);
      const videoSrc = this.options[i].src;
      this.$refs.options[i].playVideo();
      this.test();
    },
    setFullscreen(i) {
      return new Promise((resolve) => {
        this.isFullscreen = true;
        this.options[i].fullscreen = true;
        setTimeout(() => {
          resolve();
        }, 2000);
      });
    },
    test() {
      // add eventListener for keydown
      document.addEventListener("keydown", (e) => {
        switch (e.keyCode) {
          case 37: //LEFT arrow
            console.log(e.keyCode);
            break;
          case 38: //UP arrow
            console.log(e.keyCode);
            break;
          case 39: //RIGHT arrow
            break;
          case 40: //DOWN arrow
            break;
          case 13: //OK button
            break;
          case 10009: //RETURN button
            if (this.isFullscreen) {
              this.isFullscreen = false;
              console.log(this.isFullscreen);
            } else {
              tizen.application.getCurrentApplication().exit();
            }

            break;
          default:
            console.log("Key code : " + e.keyCode);
            break;
        }
      });
    },
  },
  computed: {},
  mounted() {},
};
</script>
<style lang="scss" scoped>
h1 {
  color: white;
}
.home {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  padding: 10px;
  height: 100vh;
  background: #ddd url(../assets/images/cover.jpg) repeat top left;
  .video-wrapper,
  .video,
  .video > div {
    flex: 1 1 auto;
    text-align: center;
    margin: 10px;
  }
  .video-wrapper {
    outline: none;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
    height: 350px;
    background: white;
    opacity: 0.7;
    background-repeat: no-repeat;
    background-size: cover;
    &.focus {
      opacity: 1;
      box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25),
        0 10px 10px rgba(0, 0, 0, 0.22);
    }
  }
  &.fullscreen {
    padding: 0;
    opacity: 1;
  }
  .video-wrapper.fullscreen,
  .video-wrapper.fullscreen > div {
    position: absolute;
    z-index: 50;
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
    border: 0;
    opacity: 1;
  }
}
</style>
