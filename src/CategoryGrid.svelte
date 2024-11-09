<script>
    export let categories = [
        { title: "Community", items: ["Activities", "Artists", "Events", "Pets"] },
        { title: "Housing", items: ["Apartments", "Housing Swap", "Office Rentals"] },
        { title: "Jobs", items: ["Accounting", "Engineering", "Retail"] },
        // add more categories here
    ];
</script>

<style>
    .sidebar {
        width: 20%;
        background-color: #f4f4f4;
        padding: 1rem;
    }
    .category-item {
        position: relative;
        padding: 1rem;
        cursor: pointer;
    }
    .subcategories {
        position: absolute;
        top: 0;
        left: 100%;
        background-color: #fff;
        padding: 1rem;
        display: none;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    .category-item:hover .subcategories {
        display: block;
    }
    .subcategories ul {
        list-style: none;
        padding: 0;
    }
    .subcategories li {
        margin: 0.5rem 0;
    }
</style>

<div class="sidebar">
    {#each categories as category}
        <div class="category-item">
            {category.title}
            <div class="subcategories">
                <ul>
                    {#each category.items as item}
                        <li>{item}</li>
                    {/each}
                </ul>
            </div>
        </div>
    {/each}
</div>
