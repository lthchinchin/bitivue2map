<template>
  <div class="container">
    <gmap-map
      :center="center"
      @zoom_changed="zoomingMap"
      @center_changed="changeCenter"
      mapTypeId="roadmap"
      :options="{
        backgroundColor: 'blue',
      }"
      :zoom="14"
      style="width: 100%; height: 500px"
      ref="gmap"
    >
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
        @closeclick="infoWinOpen = false"
      >
        <!-- <gmap-info-window
        :options="infoOptions"
        :position="infoWindowPos"
        :opened="infoWinOpen"
        @closeclick="infoWinOpen = false"
      > -->
      </gmap-info-window>

      <gmap-marker
        :key="i"
        v-for="(m, i) in markers"
        :position="m.position"
        :clickable="true"
        @click="toggleInfoWindow(m, i)"
      ></gmap-marker>
    </gmap-map>
  </div>
</template>
<style scoped>
.vue-map-container {
  width: 100% !important;
  height: 100vh !important;
}
</style>
<script>
export default {
  created() {
    this.$refs.gmap.setCenter({ lat: 15.996332, lng: 108.255989 });
  },
  mounted() {},
  data() {
    return {
      center: {
        lat: 15.996332,
        lng: 108.255989,
      },
      infoWindowPos: null,
      infoWinOpen: false,
      currentMidx: null,

      infoOptions: {
        content: "",
        //optional: offset infowindow so it visually sits nicely on top of our marker
        pixelOffset: {
          width: 0,
          height: -35,
        },
      },
      zoomList: [],
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
    };
  },
  computed: {},
  methods: {
    toggleInfoWindow: function (marker, idx) {
      this.infoWindowPos = marker.position;
      this.infoOptions.content = marker.infoText;

      //check if its the same marker that was selected if yes toggle
      if (this.currentMidx == idx) {
        this.infoWinOpen = !this.infoWinOpen;
      }

      //if different marker set infowindow to open and reset current marker index
      else {
        this.infoWinOpen = true;
        this.currentMidx = idx;
      }
    },
    zoomingMap(e) {
      console.log("zoom", e);
      if (e > 17) {
        // this.zoomList = this.markers;
        this.infoWinOpen = true;
      } else {
        this.infoWinOpen = false;
      }
    },
    changeCenter() {
      //   console.log("center new lalong", e.lat());
      //   this.center = { lat: e.lat(), lng: e.lng() };
    },
  },
};
</script>

<style>
</style>