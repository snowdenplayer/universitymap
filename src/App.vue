<template>
  <div id="app">
    <div style="height: 500px; width: 100%">
      <div style="height: 50px; overflow: auto;">
        <p>{{ currentObj.name }} marker is placed at {{ currentObj.position.lat }}, {{ currentObj.position.lng }}</p>

      </div>
      <l-map
          :zoom="zoom"
          :center="currentObj.position"
          :options="mapOptions"
          style="height: 80%"
          @update:center="centerUpdate"
          @update:zoom="zoomUpdate"
      >
        <l-tile-layer
            :url="url"
            :attribution="attribution"
        />
        <l-marker
            v-for="item in markers"
            :lat-lng="item.position"
            :key="item.id"
            @click="innerClick(item)"
        >
          <l-popup>
            <div>
              {{ item.tooltip + ' ' + item.position.lat + ' ' + item.position.lng }}
            </div>
          </l-popup>
        </l-marker>
      </l-map>
    </div>
  </div>
</template>

<script>
import {latLng} from "leaflet";
import {LMap, LTileLayer, LMarker, LPopup} from "vue2-leaflet";


export default {
  name: 'App',
  data() {
    return {
      zoom: 5,
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
          '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      currentObj: {
        name: 'Kiev',
        position: latLng(50.45466, 30.5238)
      },
      currentZoom: 11.5,
      mapOptions: {
        zoomSnap: 0.5
      },
      markers: [
        {
          id: 'm1',
          position: {lat: 50.45466, lng: 30.5238},
          tooltip: 'KIEV',
          draggable: true,
          visible: true,
        },
        {
          id: 'm2',
          position: {lat: 49.988358, lng: 36.232845},
          tooltip: 'KHARKIV',
        },
        {
          id: 'm3',
          position: {lat: 46.482952, lng: 30.712481},
          tooltip: 'ODESSA',
        },
        {
          id: 'm4',
          position: {lat: 49.842957, lng: 24.031111},
          tooltip: 'LVIV',
        },
        {
          id: 'm5',
          position: {lat: 48.450001, lng: 34.983334},
          tooltip: 'DNIPRO',
        },
        {
          id: 'm6',
          position: {lat: 48.023, lng: 37.80224},
          tooltip: 'DONETSK',
        },
      ],
    }
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentObj.position = center;
    },
    innerClick(el) {
      this.currentObj.name = el.tooltip
      this.currentObj.position = latLng(el.position.lat, el.position.lng)
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
