<template>
  <div class="container">
    <div>
      <nav>
        <div class="header">
          <v-card class="card">
            <v-btn :color="overview ? 'red' : ''" text @click="openOverview"
              >OVERVIEW</v-btn
            >
            <v-btn :color="mapview ? 'red' : ''" text @click="openMapView"
              >MAP VIEW</v-btn
            >
          </v-card>
        </div>
      </nav>
    </div>
    <div class="main">
      <DashboardOverview v-if="overview"></DashboardOverview>
      <!-- <OverviewContainer v-if="overview"></OverviewContainer> -->
      <mapview-container v-if="mapview"></mapview-container>
    </div>
  </div>
</template>

<script>
// import OverviewContainer from "@/components/Overview/OverviewContainer.vue";
import DashboardOverview from "@/components/Overview/DashboardOverview.vue";
import MapviewContainer from "@/components/Mapview/MapviewContainer.vue";
export default {
  components: { 
    // OverviewContainer, 
    MapviewContainer , 
    DashboardOverview },
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
    },
  },
};
</script>

<style scoped>
.container {
  background-color: rgb(244, 236, 253);
}
</style>
