<template>
  <div
    class="song w-full flex justify-start items-center py-2 pl-4  "
    :class="
      track && playbackState.item && track.uri == playbackState.item.uri
        ? 'bg-blackLight rounded-md'
        : ''
    "
  >
    <img
      class="rounded-lg mr-4 w-10 h-10"
      :src="track.album.images[2].url"
      alt=""
    />
    <div class="artist-title">
      <p class="mr-4 text-sm font-thin">
        {{ track.artists.map(artist => artist.name).join(", ") }}
      </p>
      <p class="mr-4 text-mg">{{ track.name }}</p>
    </div>
    <div class="buttons ml-auto flex justify-center items-center">
      <button
        class="bg-green mr-2 px-2  rounded-full shadow-2xl"
        @click="
          () => {
            play(track.uri);
          }
        "
      >
        >
      </button>
      <button class="bg-green px-2  rounded-full shadow-2xl">
        Q
      </button>
    </div>
  </div>
</template>

<script>
import { spotify } from "@/plugins/spotify";

export default {
  props: ["track"],
  methods: {
    play(uri) {
      spotify.play({
        uris: [uri]
      });
    }
  },
  computed: {
    playbackState() {
      return this.$store.getters.getCurrentPlayback();
    }
  }
};
</script>

<style></style>
