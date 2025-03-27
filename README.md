<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Konum ve Etrafındaki Yerler</title>
    <script src="https://maps.googleapis.com/maps/api/js?key=8906fa275e2e3d6850d757ad7dad6c5e14388282&libraries=places"></script>
    <script>
        function initMap() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(function(position) {
                    var userLocation = { lat: position.coords.latitude, lng: position.coords.longitude };
                    var map = new google.maps.Map(document.getElementById('map'), {
                        center: userLocation,
                        zoom: 15
                    });

                    var marker = new google.maps.Marker({
                        position: userLocation,
                        map: map,
                        title: "Sizin Konumunuz"
                    });

                    var request = {
                        location: userLocation,
                        radius: '1000',
                        type: ['restaurant', 'tourist_attraction'] // Etrafındaki restoranlar ve turistik yerler
                    };

                    var service = new google.maps.places.PlacesService(map);
                    service.nearbySearch(request, function(results, status) {
                        if (status === google.maps.places.PlacesServiceStatus.OK) {
                            for (var i = 0; i < results.length; i++) {
                                var place = results[i];
                                new google.maps.Marker({
                                    position: place.geometry.location,
                                    map: map,
                                    title: place.name
                                });
                            }
                        } else {
                            console.log('Hata: ' + status);
                        }
                    });
                }, function(error) {
                    console.log('Konum alınırken bir hata oluştu:', error);
                    alert("Konum alırken bir sorun oluştu.");
                });
            } else {
                alert("Tarayıcınızda konum servisi desteklenmiyor.");
            }
        }
    </script>
</head>
<body onload="initMap()">
    <h1>Konumunuzu ve Etrafınızdaki Güzel Yerleri Görün</h1>
    <div id="map" style="height: 500px; width: 100%;"></div>
</body>
</html>


<template>
  <div>
    <GmapMap
      :center="center"
      :zoom="12"
      style="width: 100%; height: 500px"
    >
      <GmapMarker :position="center" />
    </GmapMap>
  </div>
</template>

<script>
import { GmapMap, GmapMarker } from 'vue2-google-maps'

export default {
  data() {
    return {
      center: { lat: 37.7749, lng: -122.4194 }, // Varsayılan konum
    }
  },
  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition((position) => {
        this.center = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        }
      })
    }
  },
  components: {
    GmapMap,
    GmapMarker
  }
}
</script>

