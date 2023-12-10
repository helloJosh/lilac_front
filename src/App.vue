<template>
  <div class="bg">
    <div class="map-controller">
      <div class="my-location">
        <button type="submit" @click="updateCenter"><span>현재위치</span></button>
      </div>
      <div class="button-group-zoom">
        <button type="submit" @click="zoomIn"><span>줌인</span></button>
      </div>
      <div class="button-group-zoom">
        <button type="submit" @click="zoomOut"><span>줌아웃</span></button>
      </div>
    </div>
    <div class="searchbar">
      <input type="search" placeholder="동/읍/면을 입력해주세요">
      <button type="submit"><span>검색</span></button>
    </div>
    <div class="map" ref="map" id ="map">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      map: null,
      zoom: 14,
      latitude: 37.3595704,
      longitude: 127.105399,
      marker: null,
      infowindow: null,
      contentString:  '<div class="info"><p>TEST</p></div>',
    };
  },

  mounted() {
    // Load the Naver Maps JavaScript API
    const script = document.createElement("script");
    script.src = "https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=bdzpv5joha";
    script.async = true;
    script.onload = () => this.initializeMap();
    document.head.appendChild(script);
  },

  methods: {
    initializeMap() {
      // Initialize the map
      const mapOptions = {
        center: new window.naver.maps.LatLng(this.latitude, this.longitude),
        zoom: this.zoom,
      };

      this.map = new window.naver.maps.Map(this.$refs.map, mapOptions);

      this.marker = new window.naver.maps.Marker({
        map: this.map,
        position: new window.naver.maps.LatLng(this.latitude, this.longitude),
      });
      
      this.infowindow = new window.naver.maps.InfoWindow({
        content: this.contentString
      });

      window.naver.maps.Event.addListener(this.marker, 'click', (e) => {
        if (this.infowindow.getMap()) {
          this.infowindow.close();
        } else {
          this.infowindow.open(this.map, this.marker);
        }
      });


    },
    zoomIn() {
      this.zoom += 1;
      this.map.setZoom(this.zoom);
    },
    zoomOut() {
      this.zoom = Math.max(1, this.zoom - 1);
      this.map.setZoom(this.zoom);
    },
    updateCenter() {
      // Update the center coordinates based on the input values
      const newCenter = new window.naver.maps.LatLng(this.latitude, this.longitude);
      this.map.setCenter(newCenter);
    },
  },
}; 
</script>

<style>
  body{
    margin : 0;
    padding : 0;
  }
  div{
  }
  .bg{
    position: relative;
  }
  .map{
    width: 100%;
    height: 100vh;
    margin: 0;
    padding : 0;
    position: absolute;
    z-index: 1;
  }
  .searchbar{
    display: flex;
    margin-top: 30px;
    margin-left : 30px;
    position: absolute;
    z-index: 2;
  }
  .map-controller{
    display: flex;
    flex-direction: column;
    margin-top: 30px;
    right: 10%;
    position: absolute;
    z-index: 2;
    align-items: center;
  }
  .info{
    width: 200px;
    height: 200px;
    display: flex;
    position: absolute;
    z-index: 1;
    margin: 0;
    padding : 0;
  }
</style>