<script>
  let categories = [
  { name: "Community", icon: "fa fa-users", subcategories: [{ name: "Dresses", url: "/women/dresses" }, { name: "Shoes", url: "/women/shoes" }, { name: "Accessories", url: "/women/accessories" }] },
  { name: "Events", icon: "fa fa-calendar", subcategories: [{ name: "Shirts", url: "/men/shirts" }, { name: "Pants", url: "/men/pants" }, { name: "Accessories", url: "/men/accessories" }] },
  { name: "For Sale", icon: "fa fa-tags", subcategories: [{ name: "Mobile", url: "/electronics/mobile" }, { name: "Laptops", url: "/electronics/laptops" }, { name: "Tablets", url: "/electronics/tablets" }] },
  { name: "Gigs", icon: "fa fa-music", subcategories: [{ name: "Puzzles", url: "/toys/puzzles" }, { name: "Dolls", url: "/toys/dolls" }, { name: "Cars", url: "/toys/cars" }] },
  { name: "Housing", icon: "fa fa-home", subcategories: [{ name: "Consoles", url: "/gaming/consoles" }, { name: "PC Games", url: "/gaming/pc-games" }, { name: "Accessories", url: "/gaming/accessories" }] },
  { name: "Jobs", icon: "fa fa-briefcase", subcategories: [{ name: "Clutches", url: "/handbags/clutches" }, { name: "Totes", url: "/handbags/totes" }, { name: "Backpacks", url: "/handbags/backpacks" }] },
  { name: "Resumes", icon: "fa fa-file-alt", subcategories: [{ name: "Furniture", url: "/home/furniture" }, { name: "Decor", url: "/home/decor" }, { name: "Kitchen", url: "/home/kitchen" }] },
  { name: "Services", icon: "fa fa-cogs", subcategories: [{ name: "Clothing", url: "/vintage/clothing" }, { name: "Jewelry", url: "/vintage/jewelry" }, { name: "Decor", url: "/vintage/decor" }] },
  { name: "All", icon: "fa fa-th-large", subcategories: [{ name: "Clothing", url: "/vintage/clothing" }, { name: "Jewelry", url: "/vintage/jewelry" }, { name: "Decor", url: "/vintage/decor" }] },
];


  // Function to handle redirection
  function redirectToPage(url) {
    window.location.href = url;
  }
</script>

<style>
  /* Navbar container */
  .navbar {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f0eef2;
    padding: 10px 0;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    position: sticky;
    top: 106px;
    z-index: 1000;
  }

  /* Category items within the navbar */
  .navbar-item {
    flex: 0 0 auto;
    margin: 0 15px;
    text-align: center;
    color: #000;
    cursor: pointer;
    position: relative;
  }

  /* Icons in each category item */
  .navbar-item i {
  font-size: 24px;
  background: linear-gradient(125deg, rgb(85, 26, 139), rgb(206, 68, 164));
  -webkit-background-clip: text; /* For Chrome, Safari */
  background-clip: text; /* For Firefox */
  color: transparent; /* Makes the icon text transparent so the gradient shows */
  margin-bottom: 5px;
  display: block;
}

.navbar-item i:hover {
  color: #f1c40f; /* Change to a different color on hover */
  transform: scale(1.2); /* Slightly increase size on hover */
}

  .navbar-item div {
    font-size: 14px;
    font-weight: bold;
  }

  /* Subcategory dropdown styling */
  .dropdown {
    display: none;  /* Hidden by default */
    position: absolute;
    top: 100%;  /* Positioned below the category */
    left: 50%;
    transform: translateX(-50%);
    background-color: #fff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    border-radius: 5px;
    padding: 10px;
    z-index: 1000;
    width: 300px;
  }

  /* Show dropdown on hover */
  .navbar-item:hover .dropdown {
    display: block;
  }

  .subcategory-item {
    padding: 5px 0;
    text-align: left;
    color: #333;
    cursor: pointer;
    font-size: 14px;
    transition: background-color 0.2s;
  }

  .subcategory-item:hover {
    background-color: #f0f0f0;
  }
</style>

<div class="navbar">
  {#each categories as category}
    <div class="navbar-item">
      <div class="category-icon"><i class={category.icon}></i></div>
      
      <div>{category.name}</div>

      <!-- Dropdown for subcategories -->
      <div class="dropdown">
        {#each category.subcategories as subcategory}
          <div class="subcategory-item" on:click={() => redirectToPage(subcategory.url)}>
            {subcategory.name}
          </div>
        {/each}
      </div>
    </div>
  {/each}
</div>
