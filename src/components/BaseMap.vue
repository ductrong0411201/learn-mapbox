<template>
  <div class="main">
    <div class="flex">
      <!-- Map Display here -->
      <div class="map-holder">
        <div id="map"></div>
      </div>
    </div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
// import axios from "axios";
import MapboxGeocoder from "@mapbox/mapbox-gl-geocoder";
import "@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css";
import RulerControl from "mapbox-gl-ruler-control";
export default {
  data() {
    return {
      loading: false,
      location: "",
      access_token: process.env.VUE_APP_MAP_ACCESS_TOKEN,
      center: [105.3230291, 20],
      map: {},
      marker: null,
      markerstyle: {
        draggable: true,
        color: "#D80739",
      },
    };
  },

  mounted() {
    this.createMap();
  },

  methods: {
    async createMap() {
      try {
        mapboxgl.accessToken = this.access_token;
        this.map = new mapboxgl.Map({
          container: "map",
          style: "mapbox://styles/mapbox/streets-v11",
          center: this.center,
          zoom: 8,
        });
        let geocoder = new MapboxGeocoder({
          accessToken: this.access_token,
          mapboxgl: mapboxgl,
          marker: true,
        });

        this.map.addControl(geocoder);
        this.map.addControl(new mapboxgl.ScaleControl(), "bottom-right");
        this.map.addControl(new mapboxgl.NavigationControl(), "bottom-right");
        const ruler = new RulerControl();
        this.map.addControl(ruler, "bottom-right");
        this.map.addControl(
          new mapboxgl.GeolocateControl({
            positionOptions: {
              enableHighAccuracy: true,
            },
            trackUserLocation: true,
            showUserHeading: true,
          }),
          "bottom-right"
        );

        geocoder.on("result", (e) => {
          const marker = new mapboxgl.Marker(this.markerstyle)
            .setLngLat(e.result.center)
            .addTo(this.map);
          this.center = e.result.center;

          marker.on("dragend", (e) => {
            this.center = Object.values(e.target.getLngLat());
          });
        });
        this.map.on("click", this.handleMapClick);
      } catch (err) {
        console.log("map error", err);
      }
    },
    handleMapClick(event) {
      if (this.marker) {
        this.marker.remove();
      }

      const { lngLat } = event;
      this.marker = new mapboxgl.Marker(this.markerstyle)
        .setLngLat(lngLat)
        .addTo(this.map);
    },
  },
};
</script>

<style scoped>
.flex {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.map-holder {
  width: 100%;
}
#map {
  min-height: calc(100vh - 128px);
  width: 100%;
}
</style>
