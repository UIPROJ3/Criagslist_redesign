<script>
  import Sidebar from './Sidebar.svelte';
  import CategoryGrid from './CategoryGrid.svelte';
  import Footer from './Footer.svelte';
</script>

<style>
  .container {
      display: flex;
      flex-direction: column;
      min-height: 100vh;
      font-family: Arial, sans-serif;
  }
  header {
      background-color: #4CAF50;
      color: white;
      padding: 1rem;
      text-align: center;
      font-size: 1.5rem;
  }
  .content {
      display: flex;
      flex: 1;
  }
</style>
<main>
  <div class="container">
    <header>
        <input type="text" placeholder="Search Craigslist" />
    </header>
    <div class="content">
        <Sidebar />
        <CategoryGrid />
    </div>
    <Footer />
  </div>
</main>

