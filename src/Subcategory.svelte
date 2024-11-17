<script>
    import { Router, Link } from 'svelte-routing';
    export let headerHeight; // Import the store
    import data from './Catergories.json';
    let links = [
        { name: 'Home', path: '/' },
        { name: 'Dresses', path: '/dresses' },
        { name: 'Electronics', path: '/electronics' },
        { name: 'Furniture', path: '/furniture' },
        { name: 'Clothing', path: '/clothing' },
        { name: 'Contact Us', path: '/contact' }
    ];

    let isOpen = true; // Sidebar toggle state (collapsed or open)
    

    
    // Function to toggle the side nav visibility
    

    let latestEntries = [
        { title: 'New Post 1', description: 'Description for post 1', url: '/post1', imageUrl: 'https://th.bing.com/th/id/R.71d6e4a4a6af50a79d703c3497a59fd4?rik=nUdGL7dKySrZMA&riu=http%3a%2f%2fpngimg.com%2fuploads%2ftv%2ftv_PNG39283.png&ehk=tfjPnaTFysWqNQUTpisRtLCrLMEvGF6aYDl8EywP6Fo%3d&risl=1&pid=ImgRaw&r=0' },
        { title: 'New Post 2', description: 'Description for post 2', url: '/post2', imageUrl: 'https://th.bing.com/th/id/R.9f470c1ea9868b206be744756fe071d9?rik=MAWhTt9BgYKQOg&riu=http%3a%2f%2f4.bp.blogspot.com%2f-CpCgJnQsY04%2fUZ1suwOlNJI%2fAAAAAAAAEP8%2fR06pW0W7xNY%2fs1600%2flg14.jpg&ehk=FQshttk439sr2Q98M4kM2B3%2bMsO4%2fLgEUhWmhTebmSg%3d&risl=&pid=ImgRaw&r=0' },
        { title: 'New Post 3', description: 'Description for post 3', url: '/post3', imageUrl: 'https://via.placeholder.com/200' }
    ];
    
  function redirectToPage(url) {
    window.location.href = url;
  }
</script>

<div class="layout">
    <!-- Sidebar -->
    <aside class="side-nav" class:collapsed={!isOpen} style="top:{headerHeight}">
    <label for="bestSellerSort">Sort by Newest</label>
        <select id="bestSellerSort" name="bestSellerSort">
          <option value="Newest">Newest</option>
          <option value="Oldest">Oldest</option>
        </select>   
        
        
    </aside>

    <!-- Content -->
    <div class="content">
        
           
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
</div>

<style>
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }
    label{
        color:purple;
    }

    body {
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        margin: 0;
    }

    .layout {
        display: flex;
        height: 100vh;
        overflow: hidden;
    }

    /* Sidebar */
    .side-nav {
        min-width:200px;
        background-color:white;
        color: #ecf0f1;
        padding: 20px;
        position: relative;
        height: 100%;
        transition: transform 0.3s ease-in-out;
        z-index: 1;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.6);
    
    }

    .side-nav.collapsed {
        transform: translateX(-250px);
    }

    /* Sidebar Header */
    .side-nav-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 1.5rem;
        font-weight: bold;
        margin-bottom: 20px;
        color: #ecf0f1;
    }

    .logo {
        color: #ecf0f1;
    }

    .toggle-btn {
        background: none;
        border: none;
        font-size: 1.5rem;
        cursor: pointer;
        color: #ecf0f1;
    }

    /* Navigation links */
    .nav-list {
        list-style-type: none;
        padding: 0;
        margin: 0;
    }

    .nav-item {
        margin-bottom: 15px;
    }

    .nav-link {
        text-decoration: none;
        color: #bdc3c7;
        font-size: 1.2rem;
        padding: 10px;
        display: block;
        transition: background-color 0.2s ease;
    }

    .nav-link:hover {
        background-color: #34495e;
        color: #ecf0f1;
        font-weight: bold;
    }

    /* Content */
    .content {
        flex-grow: 1;
        
        padding: 20px;
        overflow-y: auto;
        transition: margin-left 0.3s ease-in-out;
    }

    .side-nav.collapsed ~ .content {
        margin-left: 0;
    }

    /* Latest Entries Section */
    .latest-entries h2 {
        color: #34495e;
        margin-bottom: 20px;
    }

    .card-container {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
        gap: 20px;
    }

    .card {
        background-color: #fff;
        padding: 15px;
        border-radius: 8px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        transition: transform 0.3s ease;
    }

    .card:hover {
        transform: translateY(-5px);
    }

    .card-image {
        width: 100%;
        height: auto;
        border-radius: 8px;
    }

    .card-title {
        display: block;
        margin-top: 10px;
        font-weight: bold;
        color: #2c3e50;
        text-decoration: none;
    }

    .card-description {
        color: #7f8c8d;
        margin-top: 10px;
    }
    /* Parent container */
.category-card {
    background-color: #34495e; /* Dark background for the card */
    color: #ecf0f1; /* Light text for contrast */
    padding: 15px; /* Padding for spacing */
    border-radius: 8px; /* Rounded corners */
    margin-bottom: 15px; /* Space between cards */
    display: flex;
    flex-direction: column;
    cursor: pointer; /* Pointer cursor for interaction */
    transition: background-color 0.3s ease, transform 0.2s ease; /* Smooth hover effect */
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* Subtle shadow for depth */
}

.category-card:hover {
    background-color: #2c3e50; /* Slightly darker on hover */
    transform: translateY(-3px); /* Lift effect on hover */
}

/* Icon inside category card */
.category-card i {
    font-size: 1.5rem; /* Icon size */
    margin-bottom: 10px; /* Space below the icon */
    color: #1abc9c; /* Accent color for icon */
}

/* Category name */
.category-card > div:first-child {
    font-size: 1.2rem; /* Slightly larger font for category name */
    font-weight: bold; /* Emphasize category name */
    margin-bottom: 10px; /* Space below the category name */
}

/* Dropdown for subcategories */
.dropdown {
    margin-top: 10px; /* Space above the dropdown */
    padding-left: 15px; /* Indent dropdown items */
    display: none; /* Hidden by default */
    flex-direction: column;
    gap: 5px; /* Space between dropdown items */
}

.category-card:hover .dropdown {
    display: flex; /* Show dropdown on hover */
}

/* Subcategory items */
.subcategory-item {
    font-size: 0.9rem; /* Smaller font for subcategories */
    color: #bdc3c7; /* Slightly muted color */
    padding: 5px 10px; /* Padding for clickable area */
    border-radius: 4px; /* Rounded edges */
    cursor: pointer;
    transition: background-color 0.2s ease, color 0.2s ease; /* Smooth hover effect */
}

.subcategory-item:hover {
    background-color: #1abc9c; /* Highlight color on hover */
    color: #fff; /* Contrast text on hover */
}

</style>
