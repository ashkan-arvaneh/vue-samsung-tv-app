<template>
  <div>
    <video-player
      ref="videoPlayer"
      class="vjs-custom-skin"
      :options="options"
      :globalOptions="globalOptions"
      @ready="onPlayerReady($event)"
    >
    </video-player>
  </div>
</template>

<script>
import { videoPlayer } from "vue-videojs7";
export default {
  name: "LivePlayer",
  components: {
    videoPlayer,
  },
  props: {
    options: {
      type: Object,
      required: true,
    },
    click: {
      type: Function,
      required: false,
    },
  },
  data() {
    return {
      globalOptions: {
        controls: false,
        fluid: true,
        navigationUI: "hide",
        aspectRatio: "16:9",
        muted: true,
        autoplay: false,
        poster: "https://i.ytimg.com/vi/dDMsK_1hg_c/maxresdefault.jpg",
        withCredentials: false,
        type: "application/x-mpegurl",
      },
    };
  },
  computed: {
    player() {
      return this.$refs.videoPlayer.player;
    },
  },
  methods: {
    onPlayerReady(player) {
      console.log("player ready!", player);
      this.player.play();
    },
    playVideo() {
      //console.log(this.options.src);
      const video = {
        withCredentials: false,
        type: "application/x-mpegurl",
        src: this.options.src,
      };
      this.player.reset(); // in IE11 (mode IE10) direct usage of src() when <src> is already set, generated errors,
      this.player.src(video);
      // this.player.load()
      this.player.play();
      //this.player.FullscreenToggle();
    },
  },
  mounted() {
    //this.playVideo(this.options.src);
  },
};
</script>

<style lang="scss" scoped>
.vjs-poster {
  background-size: cover;
}
</style>
