<script>
    import Sidebar from './Sidebar.svelte';
    import CategoryGrid from './CategoryGrid.svelte';
    import Footer from './Footer.svelte';
    import Dressses from './Dressses.svelte';
    import { Router, Route } from 'svelte-routing';
  
    let location = "Cincinnati";
    let latestEntries = [
      { title: 'New Post 1', description: 'Description for post 1', url: '/post1', imageUrl: 'https://via.placeholder.com/200' },
      { title: 'New Post 2', description: 'Description for post 2', url: '/post2', imageUrl: 'https://via.placeholder.com/200' },
      { title: 'New Post 3', description: 'Description for post 3', url: '/post3', imageUrl: 'https://via.placeholder.com/200' }
    ];
  </script>
  
  
  <main>
    <div class="container">
      <header>
        <p class="header-logo"><i class="fas fa-peace"></i>Craigslist</p>
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
  
      <div class="content">
        <Sidebar />
        <CategoryGrid />
  
        
        <div class="latest-entries">
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
      </div>
  
      <Footer />
    </div>
  </main>
  
  <style>
    .container {
      display: flex;
      flex-direction: column;
      height: 100vh;
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
      z-index: 1000;
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
      color: #551A8B;
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
  </style>
  