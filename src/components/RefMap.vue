<template>
  <div>
    <!-- <button @click="setPos">Position</button> -->
    <button @click="mapMode = 'edit'">Edit</button>
    <GmapMap
      :center="center"
      @zoom_changed="zoomingMap"
      :zoom="zoom"
      style="width: 100%; height: 500px"
      ref="googleMap"
      :options="{
        zoomControl: true,
        mapTypeControl: true,
        scaleControl: false,
        streetViewControl: false,
        rotateControl: false,
        fullscreenControl: false,
        disableDefaultUi: false,
        draggable: mapDraggable,
        draggableCursor: mapCursor,
      }"
    >
      <gmap-marker
        :key="i"
        v-for="(m, i) in markers"
        :position="m.position"
        :clickable="false"
        @click="toggleInfoWindow(m, i)"
      ></gmap-marker>
      <gmap-info-window
        v-for="i in markers"
        :key="i.infoText"
        :options="{
          content: i.infoText,
          pixelOffset: {
            width: 0,
            height: -35,
          },
        }"
        :position="i.position"
        :opened="infoWinOpen"
      >
      </gmap-info-window>
      <gmap-drawing-manager
        @update:shapes="shapePoints($event)"
        v-if="mapMode === 'edit'"
        :position="toolbarPosition"
        :rectangle-options="rectangleOptions"
        :circle-options="circleOptions"
        :polyline-options="polylineOptions"
        :shapes="shapes"
        ref="drawingRef"
      />
    </GmapMap>
  </div>
</template>

<script>
import { getGoogleMapsAPI } from "gmap-vue";

export default {
  data() {
    return {
      drawingManager: null,
      center: { lat: 10, lng: 10 },
      centerCusor: null,
      zoom: 11,
      infoWinOpen: false,
      markers: [
        {
          position: {
            lat: 15.97332,
            lng: 108.245989,
          },
          infoText: "<strong>Marker 1</strong>",
        },
        {
          position: {
            lat: 15.99432,
            lng: 108.225989,
          },
          infoText: "<strong>Marker 2</strong>",
        },
        {
          position: {
            lat: 15.986332,
            lng: 108.235989,
          },
          infoText: "<strong>Marker 3</strong>",
        },
      ],
      mapMode: null,
      toolbarPosition: "TOP_CENTER",
      mapDraggable: true,
      mapCursor: null,
      shapes: [],
      rectangleOptions: {
        fillColor: "#777",
        fillOpacity: 0.4,
        strokeWeight: 2,
        strokeColor: "#999",
        draggable: false,
        editable: false,
        clickable: true,
      },
      circleOptions: {
        fillColor: "#777",
        fillOpacity: 0.4,
        strokeWeight: 2,
        strokeColor: "#999",
        draggable: false,
        editable: false,
        clickable: true,
      },
      polylineOptions: {
        fillColor: "#777",
        fillOpacity: 0.4,
        strokeWeight: 2,
        strokeColor: "#999",
        draggable: false,
        editable: false,
        clickable: true,
      },
    };
  },
  watch: {
    mapMode(newMode, oldMode) {
      if (newMode === "ready") {
        if (oldMode === "edit") {
          this.mapDraggable = true;
          this.mapCursor = null;
          return;
        }
      }

      if (newMode === "edit") {
        this.mapDraggable = false;
        this.mapCursor = "default";
      }
    },
  },
  created() {
    console.log("sss");
  },
  async mounted() {
    await this.$gmapApiPromiseLazy();

    const google = getGoogleMapsAPI();
    console.log(">>>>>>>>>>>> placesService", google.maps.places);
    console.log(">>>>>>>>>>>> $mapObject", this.$refs.googleMap.$mapObject);

    this.$refs.googleMap.$mapObject.setCenter({
      lat: 15.996332,
      lng: 108.255989,
    });
    this.$refs.googleMap.$mapObject.setZoom(10);
  },
  methods: {
    shapePoints() {
      console.log(">>>>>>>>>>>> drawingRef", this.$refs.drawingRef.finalShapes);
      console.log(">>>>>>>>>>>> drawingRef", this.$refs.drawingRef.shapes);
    },
    zoomingMap(e) {
      console.log("zoom", e);
      if (e > 11) {
        // this.center = this.centerCusor;
        this.infoWinOpen = true;
      } else {
        this.infoWinOpen = false;
      }
    },
    setPos() {
      const posTypes = [
        "TOP_CENTER",
        "TOP_LEFT",
        "TOP_RIGHT",
        "LEFT_TOP",
        "RIGHT_TOP",
        "LEFT_CENTER",
        "RIGHT_CENTER",
        "LEFT_BOTTOM",
        "RIGHT_BOTTOM",
        "BOTTOM_CENTER",
        "BOTTOM_LEFT",
        "BOTTOM_RIGHT",
      ];

      this.toolbarPosition =
        posTypes[Math.floor(Math.random() * posTypes.length)];
    },
  },
};
</script>