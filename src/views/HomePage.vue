<template>
  <div class="container">
    <div>
      <nav>
        <div class="header">
          <v-card class="card">
            <v-btn text @click="openOverview">OVERVIEW</v-btn>
            <v-btn text @click="openMapView">MAP VIEW</v-btn>
          </v-card>
        </div>
      </nav>
    </div>
    <div class="main">
      <OverviewContainer v-if="overview"></OverviewContainer>
      <map-overview v-if="mapview"></map-overview>
    </div>
  </div>
</template>

<script>
import OverviewContainer from "@/components/Overview/OverviewContainer.vue";
import MapOverview from "@/components/MapOverview.vue";
export default {
  components: { OverviewContainer, MapOverview },
  data: () => {
    return {
      mapview: false,
      overview: true,
    };
  },
  methods: {
    openMapView() {
      this.mapview = true;
      this.overview = false;
    },
    openOverview() {
      this.mapview = false;
      this.overview = true;
    }
  },
};
</script>

<style scoped>
.container {
  background-color: rgb(244, 236, 253);
}
</style>
