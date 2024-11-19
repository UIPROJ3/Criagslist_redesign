<script>
    let title = '';
    let description = '';
    let category = '';
    let price = '';
    let email = '';
    let errorMessage = '';
    
    function validateForm() {
      if (!title || !description || !category || !price || !email) {
        errorMessage = 'All fields are required.';
        return false;
      }
      if (isNaN(price) || price <= 0) {
        errorMessage = 'Price must be a positive number.';
        return false;
      }
      if (!/\S+@\S+\.\S+/.test(email)) {
        errorMessage = 'Invalid email address.';
        return false;
      }
      errorMessage = '';
      return true;
    }
  
    function submitForm() {
      if (validateForm()) {
        // Submit logic here
        console.log('Form submitted');
      }
    }
    
    function cancelForm() {
      title = '';
      description = '';
      category = '';
      price = '';
      email = '';
      errorMessage = '';
    }
  </script>
  
  <style>
    .form-container {
      max-width: 600px;
      margin: 0 auto;
      padding: 2rem;
      background-color: #f9f9f9;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    
    h1 {
      text-align: center;
      font-size: 1.8rem;
      margin-bottom: 1rem;
    }
    
    .input-group {
      margin-bottom: 1.5rem;
    }
    
    .input-group label {
      display: block;
      font-size: 1rem;
      font-weight: bold;
      margin-bottom: 0.5rem;
    }
    
    .input-group input,
    .input-group select,
    .input-group textarea {
      width: 100%;
      padding: 0.8rem;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 5px;
      margin-top: 0.3rem;
    }
  
    .input-group textarea {
      min-height: 150px;
    }
    
    .input-group input:focus,
    .input-group select:focus,
    .input-group textarea:focus {
      outline: none;
      border-color: #007BFF;
    }
  
    .error-message {
      color: red;
      font-size: 0.875rem;
      margin-top: 0.5rem;
    }
  
    .button-group {
      display: flex;
      justify-content: space-between;
    }
  
    .button-group button {
      padding: 0.8rem 1.5rem;
      font-size: 1rem;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
  
    .button-group button.submit {
      background-color: #007BFF;
      color: white;
      border: none;
    }
  
    .button-group button.submit:hover {
      background-color: #0056b3;
    }
  
    .button-group button.cancel {
      background-color: #f44336;
      color: white;
      border: none;
    }
  
    .button-group button.cancel:hover {
      background-color: #d32f2f;
    }
  
    @media (max-width: 768px) {
      .form-container {
        padding: 1.5rem;
      }
    }
  </style>
  
  <div class="form-container">
    <h1>Add New Post</h1>
  
    {#if errorMessage}
      <p class="error-message">{errorMessage}</p>
    {/if}
  
    <div class="input-group">
      <label for="title">Title</label>
      <input type="text" id="title" bind:value={title} placeholder="Post Title" />
    </div>
  
    <div class="input-group">
      <label for="description">Description</label>
      <textarea id="description" bind:value={description} placeholder="Describe your post"></textarea>
    </div>
  
    <div class="input-group">
      <label for="category">Category</label>
      <select id="category" bind:value={category}>
        <option value="" disabled>Select a category</option>
        <option value="Furniture">Furniture</option>
        <option value="Electronics">Electronics</option>
        <option value="Jobs">Jobs</option>
        <option value="Services">Services</option>
      </select>
    </div>
  
    <div class="input-group">
      <label for="price">Price</label>
      <input type="number" id="price" bind:value={price} placeholder="Price in USD" min="0" />
    </div>
  
    <div class="input-group">
      <label for="email">Email</label>
      <input type="email" id="email" bind:value={email} placeholder="Your email address" />
    </div>
  
    <div class="button-group">
      <button type="button" class="submit" on:click={submitForm}>Submit</button>
      <button type="button" class="cancel" on:click={cancelForm}>Cancel</button>
    </div>
  </div>
  