<template>
  <div>
    <div class="map" ref="googleMap" />
  </div>
</template>

<script>
import GoogleMapsApiLoader from 'google-maps-api-loader';

export default {
    name: 'Map',
    data() {
        return {
            google: null,
            mapConfig: {
                center: {
                    lat: 36.1581018,
                    lng: 136.7722321
                },
                zoom: 13
            }
        }
    },
    async mounted() {
        this.google = await GoogleMapsApiLoader({
        apiKey: 'AIzaSyAWn2P6KSq0O7akTgkAYCvlSy_W1hDTgNQ&'
        });
        this.initializeMap();
    },
    methods: {
        initializeMap() {
        new this.google.maps.Map(this.$refs.googleMap, this.mapConfig);
    }
  }
}
</script>

<style lang="scss" scoped>
.map {
  width: 100vw;
  height: 100vh;
}
</style>