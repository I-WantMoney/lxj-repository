<template>
  <div id="map-container" class="map-container">
    <div id="map" class="map"></div>
  </div>
</template>

<script>
import { createMap, createAmplifyGeocoder } from 'maplibre-gl-js-amplify';
import { NavigationControl } from 'maplibre-gl';

export default {
  setup() {
    const mapCreate = async () => {
      const map = await createMap({
        container: 'map',
        center: [139.7157, 35.634],
        zoom: 10,
      });
      map.addControl(createAmplifyGeocoder());
      map.addControl(new NavigationControl({ visualizePitch: true }));
    };

    mapCreate();
  },
};
</script>
