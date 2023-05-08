<template>
  <div class="main">
    <div class="flex">
      <!-- Map Display here -->
      <div class="map-holder" id="comparison-container">
        <div id="map" class="map"></div>
        <div id="map-compare" class="map"></div>
      </div>
    </div>
  </div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
// window.mapboxgl = mapboxgl
import MapboxCompare from "mapbox-gl-compare";
import "mapbox-gl-compare/dist/mapbox-gl-compare.css";
import "@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css";
import RulerControl from "mapbox-gl-ruler-control";
import MapboxDraw from "@mapbox/mapbox-gl-draw";
import "@mapbox/mapbox-gl-draw/dist/mapbox-gl-draw.css";
export default {
  props: {
    isCompare: { type: Boolean, default: false },
    isDashboard: { type: Boolean, default: false },
    isMapview: { type: Boolean, default: false },
  },
  data() {
    return {
      loading: false,
      location: "",
      access_token: process.env.VUE_APP_MAP_ACCESS_TOKEN,
      center: [103.8088623, 1.3459122],
      map: {},
      marker: null,
      markerstyle: {
        draggable: true,
        color: "#D80739",
      },
      mapCompare: {},
    };
  },
  mounted() {
    // this.createMap();
    this.createMapCompare();
  },
  computed: {
    compareMod() {
      if (this.isCompare) {
        this.createMapCompare();
      }
      return this.isCompare;
    },
  },
  methods: {
    createMap() {
      try {
        mapboxgl.accessToken = this.access_token;
        this.map = new mapboxgl.Map({
          container: "map",
          style: "mapbox://styles/mapbox/streets-v11",
          attributionControl: false,
          center: this.center,
          zoom: 10.5,
        });
        this.map.addControl(new mapboxgl.ScaleControl(), "bottom-right");
        this.map.addControl(new mapboxgl.NavigationControl(), "bottom-right");

        if (this.isMapview) {
          const ruler = new RulerControl();
          this.map.addControl(ruler, "bottom-right");
          const draw = new MapboxDraw({
            displayControlsDefault: false,
            controls: {
              polygon: true,
            },
            defaultMode: "draw_polygon",
          });
          this.map.addControl(draw, "bottom-right");
        }
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
    createMapCompare() {
      // mapboxgl.accessToken = this.access_token;
      // const mapCompare = new mapboxgl.Map({
      //   container: "map-compare",
      //   style: "mapbox://styles/mapbox/streets-v11",
      //   center: this.center,
      //   zoom: 10.5,
      // });
      // const container = "#comparison-container";
      // this.mapCompare = new mapboxCompare(this.map, mapCompare, container, {
      //   mousemove: true,
      //   slider: true,
      // });
      mapboxgl.accessToken = this.access_token;
      const beforeMap = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/mapbox/streets-v11",
        attributionControl: false,
        center: [0, 0],
        zoom: 0,
      });

      const afterMap = new mapboxgl.Map({
        container: "map-compare",
        style: "mapbox://styles/mapbox/satellite-streets-v12",
        attributionControl: false,
        center: [0, 0],
        zoom: 0,
      });
      const container = "#comparison-container";

      this.map = new MapboxCompare(beforeMap, afterMap, container, {});
    },
  },
};
</script>

<style scoped>
.flex {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
  height: 100%;
}
.map-holder {
  position: relative;
  width: 100%;
  height: 100%;
}
.main {
  height: 100%;
}
.map {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
}
</style>
