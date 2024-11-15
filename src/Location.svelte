<script>
    import { onMount } from "svelte";
  
    // Initial data (latitude, longitude, and name of some places)
    const locations = [
      { id: 1, name: "Library", lat: 40.748817, lon: -73.985428 },
      { id: 2, name: "Coffee Shop", lat: 40.740610, lon: -73.935242 },
      { id: 3, name: "Park", lat: 40.741895, lon: -73.989308 },
      { id: 4, name: "Museum", lat: 40.749825, lon: -73.987963 }
    ];
  
    let userLocation = { lat: null, lon: null };
    let nearbyLocations = [];
  
    // Haversine formula to calculate distance between two lat/lon points
    function getDistance(lat1, lon1, lat2, lon2) {
      const R = 6371; // Earth radius in km
      const dLat = (lat2 - lat1) * (Math.PI / 180);
      const dLon = (lon2 - lon1) * (Math.PI / 180);
      const a =
        Math.sin(dLat / 2) * Math.sin(dLat / 2) +
        Math.cos(lat1 * (Math.PI / 180)) *
          Math.cos(lat2 * (Math.PI / 180)) *
          Math.sin(dLon / 2) * Math.sin(dLon / 2);
      const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
      const distance = R * c; // Distance in km
      return distance;
    }
  
    // Get user's current location
    onMount(() => {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          userLocation.lat = position.coords.latitude;
          userLocation.lon = position.coords.longitude;
          findNearbyLocations();
        }, (error) => {
          console.error("Geolocation error:", error);
        });
      }
    });
  
    // Function to find nearby locations
    function findNearbyLocations() {
      if (userLocation.lat && userLocation.lon) {
        nearbyLocations = locations.map(location => {
          const distance = getDistance(userLocation.lat, userLocation.lon, location.lat, location.lon);
          return { ...location, distance };
        });
  
        // Sort locations by distance (ascending)
        nearbyLocations.sort((a, b) => a.distance - b.distance);
      }
    }
  </script>
  
  <style>
    .location {
      margin: 10px 0;
      padding: 10px;
      border: 1px solid #ddd;
    }
    .location span {
      font-weight: bold;
    }
  </style>
  
  <h2>Your Current Location:</h2>
  <p>
    {#if userLocation.lat !== null && userLocation.lon !== null}
      Latitude: {userLocation.lat}, Longitude: {userLocation.lon}
    {:else}
      Loading your location...
    {/if}
  </p>
  
  <h3>Nearby Locations:</h3>
  {#if nearbyLocations.length > 0}
    <ul>
      {#each nearbyLocations as location (location.id)}
        <li class="location">
          <p>{location.name}</p>
          <p>Distance: {location.distance.toFixed(2)} km</p>
        </li>
      {/each}
    </ul>
  {:else}
    <p>Finding nearby locations...</p>
  {/if}
  