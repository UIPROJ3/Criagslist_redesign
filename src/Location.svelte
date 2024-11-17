<script>
    import { onMount } from 'svelte';
  
    let location = "Cincinnati"; // Default location for fallback
    let userLocation = { lat: null, lon: null };
    let latestEntries = [
      { title: 'New Post 1', description: 'Description for post 1', lat: 39.1031, lon: -84.5120, url: '/post1', imageUrl: 'https://via.placeholder.com/200' },
      { title: 'New Post 2', description: 'Description for post 2', lat: 40.7128, lon: -74.0060, url: '/post2', imageUrl: 'https://via.placeholder.com/200' },
      { title: 'New Post 3', description: 'Description for post 3', lat: 34.0522, lon: -118.2437, url: '/post3', imageUrl: 'https://via.placeholder.com/200' }
    ];
    
    // Haversine formula to calculate distance between two geographical points
    function getDistance(lat1, lon1, lat2, lon2) {
      const R = 6371; // Earth radius in km
      const dLat = (lat2 - lat1) * Math.PI / 180;
      const dLon = (lon2 - lon1) * Math.PI / 180;
      const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) +
                Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
                Math.sin(dLon / 2) * Math.sin(dLon / 2);
      const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
      const distance = R * c; // Distance in km
      return distance;
    }
  
    // Get the user's current location
    onMount(() => {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            userLocation.lat = position.coords.latitude;
            userLocation.lon = position.coords.longitude;
            location = `Lat: ${userLocation.lat}, Lon: ${userLocation.lon}`;
          },
          (error) => {
            console.error(error);
            alert('Unable to retrieve your location');
          }
        );
      } else {
        alert('Geolocation is not supported by this browser.');
      }
    });
  
    // Filter posts based on proximity to the user's location
    function getNearbyPosts() {
      return latestEntries.filter(entry => {
        if (userLocation.lat && userLocation.lon) {
          const distance = getDistance(userLocation.lat, userLocation.lon, entry.lat, entry.lon);
          return distance <= 50; // Filter posts within 50 km of user's location
        }
        return false; // No location data available
      });
    }
  
    let filteredEntries = getNearbyPosts();
  </script>
  
  <main>
    <div class="content">
      <h2>Location-Based Search</h2>
      <p>Your location: {location}</p>
  
      <div class="latest-entries">
        <h3>Nearby Posts (within 50 km)</h3>
        <div class="card-container">
          {#each filteredEntries as entry}
            <div class="card">
              <img src={entry.imageUrl} alt={entry.title} class="card-image" />
              <a href={entry.url} class="card-title">{entry.title}</a>
              <p class="card-description">{entry.description}</p>
            </div>
          {/each}
          {#if filteredEntries.length === 0}
            <p>No posts found near your location.</p>
          {/if}
        </div>
      </div>
    </div>
  </main>
  
  <style>
    .content {
      display: flex;
      flex-direction: column;
      padding: 20px;
    }
  
    .latest-entries {
      padding: 20px;
      background-color: #FFFFFF;
      border-radius: 8px;
      margin-top: 20px;
    }
  
    .card-container {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
    }
  
    .card {
      background-color: #FFFFFF;
      border: 1px solid #DDD;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      width: 200px;
      padding: 15px;
      transition: transform 0.2s;
    }
  
    .card:hover {
      transform: scale(1.05);
    }
  
    .card-image {
      width: 100%;
      height: 150px;
      object-fit: cover;
      border-radius: 8px;
      margin-bottom: 10px;
    }
  
    .card-title {
      font-weight: bold;
      color: #4B2F73;
      text-decoration: none;
      font-size: 1.1rem;
      margin-bottom: 8px;
    }
  
    .card-description {
      font-size: 0.9rem;
      color: #333333;
    }
  </style>
  