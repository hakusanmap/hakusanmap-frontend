<template>
    <div id="map"></div>
</template>
<script>
export default {
    data() {
        return {
            map: null
        }
   },
   mounted() {
        const element = document.getElementById('map')
        const map = new this.$gmap.Map(element, {
            center: { lat: 35.685175, lng: 139.7528 },
            zoom: 15
        }
        this.map = map
   }
}
</script>