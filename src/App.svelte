<script>
  import Sidebar from './Sidebar.svelte';
  import CategoryGrid from './CategoryGrid.svelte';
  import Footer from './Footer.svelte';
  import EventCalender from './EventCalender.svelte';
  import Location from './Location.svelte';
  import Subcategory from './Subcategory.svelte';
  import { Router, Route, navigate } from 'svelte-routing';
  import { writable } from 'svelte/store';
  import { onMount } from 'svelte';
  import data from './Catergories.json';
    import AddPost from './AddPost.svelte';

  let location = "Cincinnati";
  let addPost = false;
  let latestEntries = [
    { title: 'New Post 1', description: 'Description for post 1', url: '/post1', imageUrl: 'https://via.placeholder.com/200' },
    { title: 'New Post 2', description: 'Description for post 2', url: '/post2', imageUrl: 'https://via.placeholder.com/200' },
    { title: 'New Post 3', description: 'Description for post 3', url: '/post3', imageUrl: 'https://via.placeholder.com/200' }
  ];
  
  let header;
  export const headerHeight = writable(0);
  let selectedCategory = JSON.parse(localStorage.getItem("selectedCategory")) || null;
  let selectedSubcategory = JSON.parse(localStorage.getItem("selectedSubcategory")) || null;
  let isDropdownOpen = false;
  let hoveredCategory = null;

  // Calculate and set header height
  const calculateHeaderHeight = () => {
    if (header) {
      headerHeight.set(header.offsetHeight);
    }
  };

  // Update breadcrumb and browser history
  
  function updateBreadcrumb(category, subcategory = null) {
    selectedCategory = category;
    selectedSubcategory = subcategory;

    // Save the selected category and subcategory to localStorage
    localStorage.setItem("selectedCategory", JSON.stringify(category));
    if (subcategory) {
      localStorage.setItem("selectedSubcategory", JSON.stringify(subcategory));
      navigate(subcategory.url); // Navigate to the subcategory's URL
      window.history.pushState(
        { category, subcategory },
        '',
        subcategory.url
      );
    } else {
      localStorage.removeItem("selectedSubcategory");
      navigate(category.url); // Navigate to the category's main page or home
      window.history.pushState({ category }, '', category.url);
    }
  }


  
  // Handle back navigation
  window.addEventListener('popstate', (event) => {
    if (event.state) {
      selectedCategory = event.state.category;
      selectedSubcategory = event.state.subcategory;
    } else {
      selectedCategory = null;
      selectedSubcategory = null;
    }
  });

  // Toggle dropdown visibility
  function toggleDropdown(category) {
    isDropdownOpen = true;
    selectedCategory = category;
  }

  function closeDropdown() {
    isDropdownOpen = false;
  }
  
  function showpost()
  {
    addPost = true;
  }
  const items = [
  { name: 'Coffee Shop', description: 'Great coffee and pastries', location: { lat: 40.7128, lon: -74.0060 }, city: 'New York' },
  { name: 'Pizza Place', description: 'Delicious pizza and pasta', location: { lat: 34.0522, lon: -118.2437 }, city: 'Los Angeles' },
  { name: 'Sushi Bar', description: 'Authentic Japanese sushi', location: { lat: 35.6895, lon: 139.6917 }, city: 'Tokyo' },
  // Add more items as needed...
];



// Main search function
function searchItems(queryText, location, radius = 60) {
  // Ensure items array exists
  if (!Array.isArray(items)) {
    throw new Error("items must be an array");
  }

  // Normalize the query text
  const lowerQuery = queryText?.toLowerCase() || "";

  // If no query is provided, return an empty array
  if (!lowerQuery) {
    return [];
  }

  // Filter items based on text match and location match
  return items.filter(item => {
    // Safely handle potential null/undefined properties
    const itemName = item?.name?.toLowerCase() || "";
    const itemDescription = item?.description?.toLowerCase() || "";
    const itemCity = item?.city?.toLowerCase() || "";

    // Check if the query text matches name or description
    const matchesText = itemName.includes(lowerQuery) || itemDescription.includes(lowerQuery);
    if (!matchesText) {
      return false; // Return early if no text match
    }

    // Check location criteria (if location is provided)
    let matchesLocation = true;
    if (location) {
      if (location.lat && location.lon) {
        // Handle lat/lon-based location matching
        if (!item.location?.lat || !item.location?.lon) {
          return false; // No valid item location
        }
        const distance = haversine(
          location.lat, location.lon,
          item.location.lat, item.location.lon
        );
        if (distance > radius) {
          return false; // Outside the radius
        }
      } else if (typeof location === "string") {
        // Handle string-based city matching
        if (!itemCity.includes(location.toLowerCase())) {
          return false; // No city match
        }
      }
    }

    // Return true only if both text and location match
    return true;
  });
}

// Example Haversine formula function for calculating distances
function haversine(lat1, lon1, lat2, lon2) {
  const toRad = angle => (Math.PI / 180) * angle;
  const R = 6371; // Earth's radius in kilometers

  const dLat = toRad(lat2 - lat1);
  const dLon = toRad(lon2 - lon1);
  const a = 
    Math.sin(dLat / 2) * Math.sin(dLat / 2) +
    Math.cos(toRad(lat1)) * Math.cos(toRad(lat2)) *
    Math.sin(dLon / 2) * Math.sin(dLon / 2);

  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
  return R * c; // Distance in kilometers
}

// Variables
let textQuery = '';
let locationQuery = '';
let searchResults = [];
let searchQuery = '';
let userLocation = null;
let search = false;

// Function to update search results
function updateSearch() {
  const location = userLocation || locationQuery; // Use user location or city query
  searchResults = searchItems(textQuery, location, 10);
  console.log(searchResults);
  console.log(textQuery);
  console.log(locationQuery);
}

// Handle Enter key press
function handleKeyPress(event) {
  if (event.key === 'Enter') {
    handleSearch();
  }
}
let suggestions = ["Apple", "Banana", "Cherry", "Date", "Eggplant", "Fig", "Grape", "Honeydew"];
  let filteredSuggestions = []; // Suggestions filtered based on input

// Handle search button click
function handleSearch() {
  const searchUrl = `/SearchResults?query=${encodeURIComponent(textQuery)}&location=${encodeURIComponent(locationQuery)}`;
  navigate(searchUrl); // Use `svelte-routing`'s `navigate`
  window.history.pushState({ textQuery, locationQuery }, '', searchUrl);
  updateSearch();
  search = true;
  filteredSuggestions = suggestions.filter((item) =>
      item.toLowerCase().includes(textQuery.toLowerCase())
    );
}
function selectSuggestion(suggestion) {
    textQuery = suggestion;
    filteredSuggestions = []; // Hide suggestions after selection
    // You can also trigger a search or perform other actions here
    console.log("Selected:", suggestion);
  }

  

// Set user location (browser geolocation)
onMount(() => {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(position => {
      userLocation = {
        lat: position.coords.latitude,
        lon: position.coords.longitude,
      };
      updateSearch(); // Update search with user location
    });
  }

 
});
onMount(() => {
  const urlParams = new URLSearchParams(window.location.search);
  textQuery = urlParams.get('query') || '';
  locationQuery = urlParams.get('location') || '';
  updateSearch();
});



  onMount(() => {
    calculateHeaderHeight();
    window.addEventListener('resize', calculateHeaderHeight);
    return () => window.removeEventListener('resize', calculateHeaderHeight);
  });
  
</script>

<Router>
  <main>
    <div class="container">
      <div class="sticky">
        <header bind:this={header}>
          <p class="header-logo"><i class="fas fa-peace"></i> Craigslist</p>
          <div class="search-container">
            <input
              type="text"
              bind:value={textQuery}
              class="search-input"
              placeholder="Search by name or description..."
              on:keydown={(event)=>handleKeyPress(event)}
            />
            {#if filteredSuggestions.length > 0}
            <ul class="suggestions-list">
              {#each filteredSuggestions as suggestion}
                <li on:click={() => selectSuggestion(suggestion)}>
                  {suggestion}
                </li>
              {/each}
            </ul>
          {/if}
            <input
              type="text"
              bind:value={locationQuery}
              class="search-input location-query"
              on:keydown={(event)=>handleKeyPress(event)}
              placeholder="Search by city or location..."
            />
            <button class="search-icon">
              <i class="fa fa-search" on:click={handleSearch}></i>
            </button>
          </div>  
          
          <div class="header-actions"> 
            <button class="icon-button" on:click={showpost}>Post</button>
            <button class="icon-button">Sign In</button>
            <button class="icon-button">Register</button>
            <button class="icon-button discussion-btn">Discussion</button>
          </div>
        </header>
        <div class="breadcrumbs-box">
          <div class="breadcrumbs">
            {#if !selectedCategory && !selectedSubcategory}
              <span class="breadcrumb" on:click={() => { 
                selectedCategory = null; selectedSubcategory = null;search=false; 
                localStorage.removeItem("selectedCategory"); localStorage.removeItem("selectedSubcategory"); 
                navigate("/"); 
                window.history.pushState({}, '', '/'); // Ensure home URL is pushed to history
              }}>
                Home</span>
                {#if search && selectedCategory === null}
                <span class="breadcrumb-separator">›</span>
               <span class="breadcrumb active" >Search Results</span>
              {/if}
              
            {:else if selectedCategory}
              <span class="breadcrumb" on:click={() => { 
                selectedCategory = null; selectedSubcategory = null; 
                localStorage.removeItem("selectedCategory"); localStorage.removeItem("selectedSubcategory"); 
                navigate("/"); 
                window.history.pushState({}, '', '/'); 
                search=false;// Ensure home URL is pushed to history
              }}>
                Home
              </span>
              
              <span class="breadcrumb-separator">›</span>
              {#if selectedSubcategory && !search}
                <span class="breadcrumb active" on:click={() => { 
                  selectedSubcategory = null; 
                  localStorage.removeItem("selectedSubcategory"); 
                  const firstSubcategoryUrl = selectedCategory.url || "/"; 
                  navigate(firstSubcategoryUrl); 
                  window.history.pushState({ category: selectedCategory, subcategory: null }, '', firstSubcategoryUrl); // Update history
                }}>
                  {selectedCategory.name}
                </span>
                <span class="breadcrumb-separator">›</span>
                <span class="breadcrumb active">
                  {selectedSubcategory.name}
                </span>
              {:else if search && (selectedCategory || selectedSubcategory)}
              <span class="breadcrumb active" >Search Results</span>
              
              {:else}
                <span class="breadcrumb active">{selectedCategory.name}</span>
              {/if}
            {/if}
          </div>
          
        </div>
        
        {#if !search}
        <div class="navbar">
          {#each data.Categories as category}
            <div
              class="category-card"
              on:click={() => updateBreadcrumb(category)}
              on:mouseenter={() => (hoveredCategory = category)}
              on:mouseleave={() => (hoveredCategory = null)}
              style:selected={selectedCategory === category ? 'background-color: yellow; color: white;' : ''}>
              <p><i class={category.icon}></i> {category.name}</p>
              {#if category.subcategories}
              <div class="dropdown">
                  {#each category.subcategories as subcategory}
                    <div class="subcategory-item"
                      on:click={(event) => {
                        event.stopPropagation();
                        updateBreadcrumb(category, subcategory);
                      }}
                    >
                      {subcategory.name}
                    </div>
                  {/each}
              </div>
              {/if}
            </div>
          
          {/each}
        </div>
        {/if}
        
      </div>
      {#if hoveredCategory}
        <div class="overlay" on:click={() => (hoveredCategory = null)}></div>
      {/if}
 
        
      
      <Route path="/" let:params>
        <div class="content">
          <div class="latest-entries">
            <h2>Nearest Job Postings</h2>
            <div class="card-container">
              {#each latestEntries as entry}
                <div class="card">
                  <img src={entry.imageUrl} alt={entry.title} class="card-image" />
                  <a href={entry.url} class="card-title">{entry.title}</a>
                  <p class="card-description">{entry.description}</p>
                </div>
              {/each}
            </div><br>
              <h2>Nearest Housing </h2>
              <div class="card-container">
                {#each latestEntries as entry}
                  <div class="card">
                    <img src={entry.imageUrl} alt={entry.title} class="card-image" />
                    <a href={entry.url} class="card-title">{entry.title}</a>
                    <p class="card-description">{entry.description}</p>
                  </div>
                {/each}
              </div><br>
              <h2>Items for Sale </h2>
              <div class="card-container">
                {#each latestEntries as entry}
                  <div class="card">
                    <img src={entry.imageUrl} alt={entry.title} class="card-image" />
                    <a href={entry.url} class="card-title">{entry.title}</a>
                    <p class="card-description">{entry.description}</p>
                  </div>
                {/each}
              </div>
            </div>
          </div>
          <EventCalender />
          
        
      </Route>

      <Route path="/Artists" let:params>
        <Subcategory {headerHeight} />
      </Route>

      <Route path="/SearchResults" let:params>
        <p>Search Results</p>
        <div class="results">
          {#if searchResults.length>0}
          {#each searchResults as item}
        
            <div class="item">
              <h3>{item.name}</h3>
              <p>{item.description}</p>
              <p>Location: {item.city}</p>
            </div>

          {/each}
          {:else}
          <p>No results found</p>
          {/if}
        </div>
        
        
      </Route>
      
    </div>
    <Footer />
  </main>
</Router>
<style>
  
  .container {
    display: flex;
    flex-direction: column;
    height: auto;
    background-color: #F8F9F9;
    font-family: Arial, sans-serif;
  }

  header {
    display: flex;
    align-items: center;
    padding: 10px 30px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    position: sticky;
    top: 0;
    z-index: 2000;
    background:linear-gradient(125deg, rgb(85, 26, 139), rgb(206 68 164));;
    -webkit-background-clip: border-box; /* For Chrome, Safari */
    background-clip: border-box; /* For Firefox */
    color: transparent;
  }
  .sticky{
    position:sticky;
    top:0;
    z-index:1000;
  }

  .header-logo {
    font-size: 1.5rem;
    font-weight: bold;
    
    color:white;
     /* Makes the text color transparent so the gradient shows */
}


  .header-actions {
    margin-left: auto;
    display: flex;
    gap: 10px;
  }

  .icon-button {
    background-color: white;
    border: none;
    border-radius: 5px;
    padding: 8px 15px;
    cursor: pointer;
    font-size: 14px;
    font-weight: bold;
  }

  .icon-button:hover {
    transform: scale(1.15);
    color: #f1c40f;
    
  }

  .discussion-btn {
    background-color:white;
  }

  .content {
    display: flex;
    flex-direction: column;
    padding: 20px;
  }

  .search-container {
    display: flex;
    align-items: center;
    
    
    margin-left: 50px;
  
  
    max-width: 600px;
    /* Remove right and add left */
    left: 0;
    position: relative; /* Ensure proper positioning */
    
}

  /* .search-input {
    border: 1px solid #DDD;
    border-radius: 8px;
    outline: none;
    flex: 1;
    padding: 10px;
    font-size: 16px;
    margin-left:20px;
  } */

  .search-icon {
    
    border: none;
    cursor: pointer;
    font-size: 20px;/* For Firefox */
    padding-right: 20px;
    background-color: none;
    background:none;
    -webkit-background-clip: border-box; /* For Chrome, Safari */
    background-clip: border-box; /* For Firefox */
    color: transparent;
   
  } 
  .search-container {
    display: flex;
    align-items: center;
  }

  .search-input {
     /* Space between input fields */
    border: 2px solid black;
    border-left:none;
    padding:15px;
  
  }

  .search-input.location-query {
    border: 2px solid black; 
    }

  /* .search-icon {
    padding: 8px;
    background-color: #f0f0f0;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
  } */

  .search-icon i {
    font-size: 30px;
    color:white;

  }
 .search-icon:hover{
  background-color: grey;
 }
  .latest-entries {
    padding: 20px;
    background-color: #FFFFFF;
    border-radius: 8px;
    margin-top: 20px;
  }

  .latest-entries h2 {
    font-size: 1.5rem;
    color: #003366;
    margin-bottom: 10px;
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
  .breadcrumbs-box {
    padding: 15px;
    background-color: #fff; /* White background for contrast */
    border: 1px solid #f0eef2; /* Light border matching navbar background */
    /* Rounded corners */
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
     /* Spacing below the breadcrumbs box */
    z-index:1000;
    position:sticky;
  
  }

  /* Breadcrumb container styles */
  .breadcrumbs {
    font-size: 16px;
    display: flex;
    align-items: center;
  }

  /* Breadcrumb links */
  .breadcrumb {
    display: inline-block;
    margin-right: 10px;
    color: rgb(206 68 164);
    cursor: pointer;
    font-weight: bold;
    transition: color 0.2s ease;
  }

  .breadcrumb:hover {
    color: #f1c40f; /* Change color on hover */
    text-decoration: underline; /* Optional: underline on hover */
  }

  .breadcrumb:active {
    color: #8e44ad; /* Active state */
  }

  /* Breadcrumb separator */
  .breadcrumb-separator {
    margin-right: 10px;
    color: #333;
  }

  /* Active breadcrumb style */
  .breadcrumb.active {
    color: purple; /* Highlight active breadcrumb */
  }

  /* Optional: Adding a background color on hover */
  .breadcrumb:hover {
    background-color: #f0f0f0;
    border-radius: 4px;
    padding: 2px 5px;
  }
  /* .navbar {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: flex-start;
    gap: 15px;
    background-color: white;
    padding-top:10px;
    padding-left:5px;
    padding-bottom: 10px;
    position: sticky;
    top:100px;
  } */

  .category-card {
    background: #fff;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.6);
    
    width: 200px;
    height: 50px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    cursor: pointer;
    position: relative;
    transition: transform 0.2s, box-shadow 0.2s;
    font-weight: bold;
  }

  .category-card:hover {
    transform: translateY(-5px); /* Lift effect */
    color: #f1c40f; /* Change text color to golden-yellow */
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15); /* Add soft shadow */
}


  .category-card i {
    font-size: 20px;
    background: linear-gradient(125deg, rgb(85, 26, 139), rgb(206, 68, 164));
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    margin-bottom: 10px;
  }
  

.category-card:hover i {
  color: #c9b2ce; /* Apply hover color to the icon */
}

  .category-card div {
    font-size: 16px;
    font-weight: bold;
    color: #333;
  }

  /* Dropdown container */
  .dropdown {
  display: none;
  position: absolute;
  top: 50px;
  left: 0; /* Align dropdown to the left edge of the page */
  min-width:300px; /* Occupy the entire width of the navbar */
  background-color: white; /* White background for dropdown */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5); /* Subtle shadow for depth */
  border-radius: 0; /* Remove border-radius for alignment */
  z-index: 1001;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s, visibility 0.3s;
  overflow-y: auto; /* Enable vertical scrolling if needed */
  overflow-x: auto; /* Disable horizontal scrolling */
  max-height: 400px; /* Limit the dropdown height */
}

.dropdown::-webkit-scrollbar {
  width: 6px; /* Set scrollbar width */
}

.dropdown::-webkit-scrollbar-thumb {
  background-color: #bcbcba; /* Set the color of the scrollbar thumb */
  border-radius: 3px; /* Rounded corners for the scrollbar thumb */
}

.dropdown::-webkit-scrollbar-thumb:hover {
  background-color: #555; /* Darker color when hovered */
}

.dropdown::-webkit-scrollbar-track {
  background: #f1f1f1; /* Light track color */
  border-radius: 3px; /* Rounded corners for the track */
}

.dropdown::-webkit-scrollbar-track:hover {
  background: #e1e1e1; /* Darker track color when hovered */
}

.dropdown::-webkit-scrollbar-corner {
  background-color: transparent; /* Remove the corner square */
}

.category-card:hover .dropdown {
  display: block;
  opacity: 1;
  visibility: visible;
}

/* Subcategory item */
.subcategory-item {
  padding: 12px 20px;
  text-align: left;
  color: #333; /* Maintain text color */
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  border-bottom: 1px solid #f0eef2; /* Separator between items */
  transition: background-color 0.2s, padding-left 0.2s;
}
  /* Subcategory hover effect */
  

  

  .subcategory-item:last-child {
  border-bottom: none;
}

/* Subcategory hover effect */
.subcategory-item:hover {
  background-color: #f7f7f7; /* Light hover background */
  color: #f1c40f; /* Change text color on hover */
  padding-left: 25px; /* Indentation effect */
}

/* Optional: Highlight the entire dropdown on hover */


/* Sticky navbar positioning */
.navbar {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
  gap: 15px;
  background-color: white;
  padding: 10px 5px;
  position: sticky;
  top: 85px;
  z-index:1000;
}
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent black background */
    z-index: 5; /* Ensure overlay is above all other content */
  }
  .suggestions-list {
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    border: 1px solid #ccc;
    border-top: none;
    background-color: #fff;
    max-height: 200px;
    overflow-y: auto;
    margin: 0;
    padding: 0;
    list-style-type: none;
    z-index:2000;
    color:black;
  }

  .suggestions-list li {
    padding: 8px;
    cursor: pointer;
  }

  .suggestions-list li:hover {
    background-color: #f0f0f0;
  }
</style>
