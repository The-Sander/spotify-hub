<template>
  <div class="wrapper py-8">
    <div class="content flex flex-col items-center justify-center w-64">
      <div
        v-if="menuActive"
        :class="menuActive ? 'opacity-100' : 'opacity-0'"
        class="rounded-md bg-green p-2 transition w-48 divide-y divide-white"
      >
        <div
          v-for="device in devices"
          :key="device.id"
          href="#"
          class="text-white cursor-pointer 
           px-4 py-2 text-sm truncate"
          @click="() => selectDevice(device)"
        >
          {{ device.name }}
        </div>
      </div>

      <input
        class="volumeSlider"
        type="range"
        min="0"
        max="100"
        v-if="playbackState.device"
        :value="playbackState ? playbackState.device.volume_percent : 50"
        @change="setVolume"
        :style="{
          backgroundImage: `linear-gradient(90deg, #1db954 ${
            playbackState ? playbackState.device.volume_percent : 50
          }% ,#333333
         ${playbackState ? playbackState.device.volume_percent : 50}% )`
        }"
      />
      <button
        class="bg-green w-32 px-8 py-2 rounded-full text-white text-lg font-bold cursor-pointer truncate"
        id="menu-button"
        aria-expanded="true"
        aria-haspopup="true"
        @click="menuActive = !menuActive"
      >
        <span v-if="selectedDevice">{{ selectedDevice.name }}</span>
        <span v-else>Device</span>
      </button>
    </div>
  </div>
</template>

<script>
import { spotify } from "@/plugins/spotify";

export default {
  data() {
    return {
      menuActive: false
    };
  },
  computed: {
    devices() {
      return this.$store.getters.getDevices();
    },
    selectedDevice() {
      return this.$store.getters.getSelectedDevice();
    },
    playbackState() {
      return this.$store.getters.getCurrentPlayback();
    }
  },
  methods: {
    selectDevice(device) {
      this.$store.dispatch("selectDevice", device);
      this.menuActive = false;
    },
    setVolume(e) {
      spotify.setVolume(e.target.value);
    }
  }
};
</script>

<style></style>
