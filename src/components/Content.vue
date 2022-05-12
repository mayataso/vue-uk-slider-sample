<template>
  <h1>
    Vue3 + TypeScript + UIkit
    <br />スライド切り替え時に色々処理を挟むサンプルです
  </h1>
  <div
    class="uk-position-relative uk-visible-toggle uk-light slider-area"
    tabindex="-1"
    uk-slider="center: true;"
    @itemshown="itemshown"
  >
    <ul
      class="uk-slider-items uk-child-width-1-2 uk-child-width-1-3@s uk-child-width-1-4@m slider-area"
    >
      <li
        v-for="(image, index) in images"
        class="uk-width-4-5"
        style="position: relative"
        :key="`img-${index}`"
      >
        <template v-if="index < 4">
          <img :src="image.path" alt="" />
        </template>
        <template v-else>
          <video id="video" :src="movie" alt="" />
        </template>
        <div class="uk-position-center uk-panel">
          <h1>{{ index + 1 }}</h1>
        </div>
        <div
          v-if="index !== 4"
          @click="setIsMute(!isMute)"
          class="uk-position-bottom-right circle-button"
        >
          <template v-if="isMute">
            <mdiVolumeMute size="40" />
          </template>
          <template v-else>
            <mdiVolumeHigh size="40" />
          </template>
        </div>
      </li>
    </ul>

    <a
      class="uk-position-center-left uk-position-small uk-hidden-hover"
      href="#"
      uk-slidenav-previous
      uk-slider-item="previous"
    ></a>
    <a
      class="uk-position-center-right uk-position-small uk-hidden-hover"
      href="#"
      uk-slidenav-next
      uk-slider-item="next"
    ></a>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, VideoHTMLAttributes } from "vue";
import UIkit from "uikit";
import "uikit/dist/css/uikit.min.css";
import mdiVolumeHigh from "vue-material-design-icons/VolumeHigh.vue";
import mdiVolumeMute from "vue-material-design-icons/VolumeMute.vue";

type AssetProf = {
  path: string;
  alt?: string;
};

interface HTMLButtonEvent extends Event {
  target: HTMLButtonElement;
}

export default defineComponent({
  name: "Content",
  props: {},
  components: {
    mdiVolumeHigh,
    mdiVolumeMute,
  },
  setup() {
    UIkit.slider(".slider-area");

    // 音声関係
    let audio = ref(new Audio(`${require("@/assets/sounds/snd1.mp3")}`));
    audio.value.volume = 0.2;
    audio.value.play();
    const isMute = ref(true);
    const setIsMute = (value: boolean) => {
      isMute.value = value;
      isMute.value ? audio.value.pause() : audio.value.play();
    };

    const videoa = ref();

    // 動画関連
    const autoPlay = ref(false);
    const playVideo = () => {
      const video: any = document.getElementById("video");
      video.play();
    };
    const stopVideo = () => {
      const video: any = document.getElementById("video");
      video.pause();
    };

    const images: AssetProf[] = [...Array(5).keys()].map(
      (n) =>
        ({
          path: require(`@/assets/images/img${n + 1}.jpeg`),
        } as AssetProf)
    );
    const movie = `${require(`@/assets/movies/mov1.mp4`)}`;

    const itemshown = (event: HTMLButtonEvent) => {
      // 簡易的にカレントコンテントの判定にli内部のテキストを使用 (アトリビュートを使ったほうが汎用的)
      const num = Number.parseInt(event.target.innerText);

      if (num === 5) {
        playVideo();
        setIsMute(true);
      } else {
        stopVideo();
        setIsMute(true);
        audio.value.currentTime = 0;
        audio.value.src = `${require(`@/assets/sounds/snd${num}.mp3`)}`;
      }
    };

    return {
      isMute,
      setIsMute,
      autoPlay,
      images,
      movie,
      videoa,
      itemshown,
    };
  },
});
</script>

<style scoped lang="scss">
img,
video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.circle-button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 60px;
  height: 60px;
  margin: 10px;
  border-radius: 50%;
  background-color: #79b3f1;
  cursor: pointer;
}
</style>
