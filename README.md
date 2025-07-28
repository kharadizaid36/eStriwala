# eStriwala
Pressing / ironing your cloths with eStriwala
<!DOCTYPE html>
<html>
<head>
  <title>eStriwala Booking App</title>
  <style>
    body { font-family: sans-serif; text-align: center; margin: 0; }
    #map { height: 400px; width: 100%; }
    #searchInput {
      width: 80%;
      padding: 10px;
      margin: 10px auto;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button { padding: 10px 15px; margin: 10px; border: none; border-radius: 5px; }
    .confirm { background-color: #ff4081; color: white; }
    .whatsapp { background-color: #25D366; color: white; }
  </style>
</head>
<body>

  <h2>🧺 eStriwala Booking</h2>
  <p>Search your location or click on the map to set pickup and drop:</p>
  <input id="searchInput" type="text" placeholder="Search location..." />
  <div id="map"></div>

  <p>
    <label><input type="checkbox" id="scheduleLater"> Schedule Later</label><br>
    <input type="datetime-local" id="scheduleTime" style="display:none; margin-top:10px;">
  </p>

  <button class="confirm" onclick="confirmBooking()">Confirm Booking</button>
  <button class="whatsapp" onclick="openWhatsApp()">WhatsApp Support</button>

  <script>
    let map, pickupMarker, dropMarker;
    let selecting = 'pickup';
    let pickup, drop, autocomplete;

    function initMap() {
      map = new google.maps.Map(document.getElementById("map"), {
        center: { lat: 28.6139, lng: 77.209 },
        zoom: 12
      });

      const input = document.getElementById("searchInput");
      autocomplete = new google.maps.places.Autocomplete(input);
      autocomplete.bindTo("bounds", map);

      autocomplete.addListener("place_changed", () => {
        const place = autocomplete.getPlace();
        if (!place.geometry) return;
        map.setCenter(place.geometry.location);
        map.setZoom(15);
      });

      map.addListener("click", (e) => {
        const pos = { lat: e.latLng.lat(), lng: e.latLng.lng() };

        if (selecting === 'pickup') {
          if (pickupMarker) pickupMarker.setMap(null);
          pickup = pos;
          pickupMarker = new google.maps.Marker({ position: pos, map, label: "P" });
          selecting = 'drop';
        } else if (selecting === 'drop') {
          if (dropMarker) dropMarker.setMap(null);
          drop = pos;
          dropMarker = new google.maps.Marker({ position: pos, map, label: "D" });
          selecting = 'done';
        }
      });

      document.getElementById("scheduleLater").addEventListener("change", function () {
        document.getElementById("scheduleTime").style.display = this.checked ? 'inline' : 'none';
      });
    }

    function confirmBooking() {
      if (!pickup || !drop) {
        alert("Please select both pickup and drop location!");
        return;
      }

      let message = `✅ Booking Confirmed!\n📍 Pickup: (${pickup.lat.toFixed(3)}, ${pickup.lng.toFixed(3)})\n📍 Drop: (${drop.lat.toFixed(3)}, ${drop.lng.toFixed(3)})`;

      if (document.getElementById("scheduleLater").checked) {
        const time = document.getElementById("scheduleTime").value;
        if (time) message += `\n🕒 Scheduled: ${time}`;
      }

      alert(message);
    }

    function openWhatsApp() {
      const number = "919975441886"; // your WhatsApp number
      const msg = "Hello, mujhe ironing service ke liye help chahiye!";
      window.open(`https://wa.me/${number}?text=${encodeURIComponent(msg)}`, "_blank");
    }
  </script>

  <script async
    src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDR0s9VHJGQWWJHpb_zkPk4ZUrVWxN-QSY&callback=initMap&libraries=places">
  </script>

</body>
</html>

