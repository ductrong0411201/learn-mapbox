<template>
  <div>
    <span class="title-map">SATELLITE MAPVIEW </span>
    <div class="mod">
      <v-card>
        <div class="btn-group">
          <v-btn
            text
            :class="isOverlay ? 'btn-mod btn-border' : 'btn-mod'"
            @click="openOverlay"
            >OVERLAY</v-btn
          >
          <v-btn
            text
            :class="isCompare ? 'btn-mod btn-border' : 'btn-mod'"
            @click="openSlide"
            >SLIDE</v-btn
          >
          <v-btn
            text
            :class="isSync ? 'btn-mod btn-border' : 'btn-mod'"
            @click="openSync"
            >SYNC</v-btn
          >
        </div>
      </v-card>
    </div>
    <v-btn class="btn-layers" icon
      ><v-icon color="white"
        >mdi-layers</v-icon
      ></v-btn
    >
    <base-map
      style="height: calc(100vh - 128px)"
      :isCompare="isCompare"
      :isMapview="isMapview"
    ></base-map>
  </div>
</template>

<script>
import BaseMap from "../BaseMap.vue";
export default {
  components: { BaseMap },
  data: () => {
    return {
      isOverlay: true,
      isCompare: false,
      isSync: false,
      isMapview: true,
    };
  },
  methods: {
    openOverlay() {
      this.isOverlay = true;
      this.isCompare = false;
      this.isSync = false;
    },
    openSlide() {
      this.isOverlay = false;
      this.isCompare = true;
      this.isSync = false;
    },
    openSync() {
      this.isOverlay = false;
      this.isCompare = false;
      this.isSync = true;
    },
  },
};
</script>

<style scoped>
.title-map {
  text-shadow: rgba(0, 0, 0, 0.4) 0px 0px 10px !important;
  z-index: 2;
  position: absolute;
  top: 62px;
  right: 25px;
  font-weight: bold;
  color: white;
  font-size: 38px;
  text-transform: uppercase;
}
.btn-group {
  display: flex;
  justify-content: center;
  height: 100%;
  align-items: center;
}
.v-card {
  height: 100%;
  border-radius: 10px !important;
}
.mod {
  height: 35px !important;
  width: 400px;
  position: absolute;
  top: 122px;
  right: 10px;
  z-index: 3;
}

.btn-mod {
  height: 26px !important;
  min-height: 0px;
  width: 85px;
  border-radius: 8px;
}
.btn-layers {
  height: 40px;
  min-width: 0px;
  width: 40px;
  background-color: rgb(137, 63, 242) !important;
  position: absolute;
  bottom: 240px;
  right: 10px;
  border-color: rgb(137, 63, 242);
  z-index: 3;
}
</style>
