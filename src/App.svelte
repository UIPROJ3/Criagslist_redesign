<script>
  import Sidebar from './Sidebar.svelte';
  import CategoryGrid from './CategoryGrid.svelte';
  import Footer from './Footer.svelte';
  import Dressses from './Dressses.svelte';
  import { Router, Route } from 'svelte-routing';
  import EventCalender from './EventCalender.svelte';
  import Location from './Location.svelte';
  import Subcategory from './Subcategory.svelte';
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import data from './Catergories.json';
  import { navigate } from 'svelte-routing';  // Import `navigate` from svelte-routing for URL changes
 
 

  let location = "Cincinnati";
  let latestEntries = [
    { title: 'New Post 1', description: 'Description for post 1', url: '/post1', imageUrl: 'https://th.bing.com/th/id/R.71d6e4a4a6af50a79d703c3497a59fd4?rik=nUdGL7dKySrZMA&riu=http%3a%2f%2fpngimg.com%2fuploads%2ftv%2ftv_PNG39283.png&ehk=tfjPnaTFysWqNQUTpisRtLCrLMEvGF6aYDl8EywP6Fo%3d&risl=1&pid=ImgRaw&r=0' },
    { title: 'New Post 2', description: 'Description for post 2', url: '/post2', imageUrl: 'https://th.bing.com/th/id/R.9f470c1ea9868b206be744756fe071d9?rik=MAWhTt9BgYKQOg&riu=http%3a%2f%2f4.bp.blogspot.com%2f-CpCgJnQsY04%2fUZ1suwOlNJI%2fAAAAAAAAEP8%2fR06pW0W7xNY%2fs1600%2flg14.jpg&ehk=FQshttk439sr2Q98M4kM2B3%2bMsO4%2fLgEUhWmhTebmSg%3d&risl=&pid=ImgRaw&r=0' },
    { title: 'New Post 3', description: 'Description for post 3', url: '/post3', imageUrl: 'https://via.placeholder.com/200' }
  ];
  let header;
  export const headerHeight = writable(0);

  // Function to calculate and set header height
  const calculateHeaderHeight = () => {
    if (header) {
      headerHeight.set(header.offsetHeight);
    }
  };
 
 

  // Retrieve saved breadcrumb state from localStorage
  


  export let selectedCategory = JSON.parse(localStorage.getItem("selectedCategory"));
  export let selectedSubcategory = JSON.parse(localStorage.getItem("selectedSubcategory"));

  // Update breadcrumb and browser history
  function updateBreadcrumb(category, subcategory = null) {
    selectedCategory = category;
    selectedSubcategory = subcategory;

    // Save breadcrumb state to localStorage
    localStorage.setItem("selectedCategory", JSON.stringify(category));
    localStorage.setItem("selectedSubcategory", JSON.stringify(subcategory));

    // Add the new state to the history stack using pushState
    const newUrl = subcategory ? subcategory.url : category.subcategories[0].url;
    window.history.pushState({ category, subcategory }, '', newUrl);

    // Update the URL using navigate
    navigate(newUrl);
  }

  // Handle back navigation using popstate
  window.addEventListener('popstate', (event) => {
    if (event.state) {
      selectedCategory = event.state.category;
      selectedSubcategory = event.state.subcategory;
    } else {
      selectedCategory = null;
      selectedSubcategory = null;
    }
    // Update breadcrumbs or any UI based on the new state
  });

  // Function to redirect to a specific page
  function redirectToPage(url) {
    window.location.href = url;
  }
  let isDropdownOpen = false;
  let hoveredCategory = null;

  // Toggle dropdown visibility
  function toggleDropdown(category) {
    isDropdownOpen = true;
    selectedCategory = category;
  }
  

  // Close the dropdown
  function closeDropdown() {
    isDropdownOpen = false;
  }

  // Recalculate height on mount and resize
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
            <input type="text" placeholder="Search Craigslist" class="search-input" />
            <button class="search-icon">
              <i class="fa fa-search"></i>
            </button>
          </div>
  
          <div class="header-actions">
            <button class="icon-button">Post</button>
            <button class="icon-button">Sign In</button>
            <button class="icon-button">Register</button>
            <button class="icon-button discussion-btn">Discussion</button>
          </div>
        </header>
        <div class="breadcrumbs-box">
          <div class="breadcrumbs">
            {#if !selectedCategory && !selectedSubcategory}
              <span class="breadcrumb" on:click={() => { 
                selectedCategory = null; selectedSubcategory = null; 
                localStorage.removeItem("selectedCategory"); localStorage.removeItem("selectedSubcategory"); 
                navigate("/"); 
                window.history.pushState({}, '', '/'); // Ensure home URL is pushed to history
              }}>
                Home
              </span>
            {:else if selectedCategory}
              <span class="breadcrumb" on:click={() => { 
                selectedCategory = null; selectedSubcategory = null; 
                localStorage.removeItem("selectedCategory"); localStorage.removeItem("selectedSubcategory"); 
                navigate("/"); 
                window.history.pushState({}, '', '/'); // Ensure home URL is pushed to history
              }}>
                Home
              </span>
              <span class="breadcrumb-separator">›</span>
              {#if selectedSubcategory}
                <span class="breadcrumb active" on:click={() => { 
                  selectedSubcategory = null; 
                  localStorage.removeItem("selectedSubcategory"); 
                  const firstSubcategoryUrl = selectedCategory.subcategories[0]?.url || "/"; 
                  navigate(firstSubcategoryUrl); 
                  window.history.pushState({ category: selectedCategory, subcategory: null }, '', firstSubcategoryUrl); // Update history
                }}>
                  {selectedCategory.name}
                </span>
                <span class="breadcrumb-separator">›</span>
                <span class="breadcrumb active">
                  {selectedSubcategory.name}
                </span>
              {:else}
                <span class="breadcrumb active">{selectedCategory.name}</span>
              {/if}
            {/if}
          </div>
        </div>
      <div class="navbar">
        {#each data.Categories as category}
          <div class="category-card" on:click={() => updateBreadcrumb(category)} on:mouseenter={() => hoveredCategory = category} on:mouseleave={() => hoveredCategory = null}>
            
            <p> <i class={category.icon}></i> {category.name}</p>
            <div class="dropdown">
              {#each category.subcategories as subcategory}
                <div class="subcategory-item" on:click={(event) => { event.stopPropagation(); updateBreadcrumb(category, subcategory); }}>
                  {subcategory.name}
                </div>
              {/each}
            </div>
          </div>
        {/each}
      </div>
      {#if isDropdownOpen || hoveredCategory}
        <div class="overlay" on:click={closeDropdown} ></div>
      {/if}
      
    </div>
      

    <Route path="/" let:params>
      <div class="content">
            
        
        
         <div class="latest-entries">
            <h2>Latest Entries</h2>
            <div class="card-container">
              {#each latestEntries as entry}
                <div class="card">
                  <img src={entry.imageUrl} alt={entry.title} class="card-image" />
                  <a href={entry.url} class="card-title">{entry.title}</a>
                  <p class="card-description">{entry.description}</p>
                  <button 
                  class="like-button {entry.liked ? 'liked' : ''}" 
                  on:click={() => likePost(post.id)}
                >
                  {entry.liked ? 'Unlike' : 'Like'}
                </button>
                </div>
              {/each}
            </div>
            <h2>Latest Entries</h2>
            <div class="card-container">
              {#each latestEntries as entry}
                <div class="card">
                  <img src={entry.imageUrl} alt={entry.title} class="card-image" />
                  <a href={entry.url} class="card-title">{entry.title}</a>
                  <p class="card-description">{entry.description}</p>
                </div>
              {/each}
            </div>
            <h2>Latest Entries</h2>
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
          
          <EventCalender/>
          <Location/>
    </Route>
    <Route path="/Community/Artists" let:params>
        <Subcategory/>
    </Route>
        
        <!-- Example Route for 'Dresses' Component -->
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
    background-color: white;
    display: flex;
    align-items: center;
    padding: 10px 30px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    position: sticky;
    top: 0;
    z-index: 100;
  }
  .sticky{
    position:sticky;
    top:0;
    z-index:1000;
  }

  .header-logo {
    font-size: 1.5rem;
    font-weight: bold;
    background:linear-gradient(125deg, rgb(85, 26, 139), rgb(206 68 164));;
    -webkit-background-clip: text; /* For Chrome, Safari */
    background-clip: text; /* For Firefox */
    color: transparent; /* Makes the text color transparent so the gradient shows */
}


  .header-actions {
    margin-left: auto;
    display: flex;
    gap: 10px;
  }

  .icon-button {
    background:linear-gradient(125deg, rgb(85, 26, 139), rgb(206 68 164));;
    -webkit-background-clip:border-box; /* For Chrome, Safari */
    background-clip:border-box; /* For Firefox */
    color: white;
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
    background-color: #4B2F73;
  }

  .content {
    display: flex;
    flex-direction: column;
    padding: 20px;
  }

  .search-container {
    display: flex;
    align-items: center;
    border: 1px solid #5F6A8A;
    border-radius: 10px;
    margin: 20px 0;
    width: 100%;
    max-width: 600px;
    /* Remove right and add left */
    left: 0;
    position: relative; /* Ensure proper positioning */
}

  .search-input {
    border: none;
    outline: none;
    flex: 1;
    padding: 10px;
    font-size: 16px;
    margin-left:20px;
  }

  .search-icon {
    
    border: none;
    cursor: pointer;
    font-size: 20px;
    background:linear-gradient(125deg, rgb(85, 26, 139), rgb(206 68 164));;
    -webkit-background-clip:text; /* For Chrome, Safari */
    background-clip:text; /* For Firefox */
    color:transparent;
    padding-right: 10px;
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
    font-size: 14px;
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
    border-radius: 8px;
    width: 200px;
    height: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    cursor: pointer;
    position: relative;
    transition: transform 0.2s, box-shadow 0.2s;
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
  top: 31px;
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
</style>
