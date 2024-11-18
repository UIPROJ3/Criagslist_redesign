<script>
  let posts = [
    { 
      id: 1, 
      title: "Cheap 1440p 120hz+ Gaming monitor wanted", 
      content: "Hi, I am building a computer, and I have spent most of my budget on the build, and I didn't budget for peripherals. I have a really old 1080p monitor that artifacts, and glitches lots, and if I use this, it will bottleneck my whole build. If you have a monitor you want to get rid of, I dont have a big budget ($80 per monitor max) I know this is asking for a lot, but if you can send me a good deal, or a link to a good sale on amazon or something I would be so grateful. Thank you!", 
      replies: [
        { user: "Alice", content: "Start by going through the official docs." },
        { user: "Bob", content: "Try building simple projects to get hands-on experience." }
      ],
      likes: 10,
      label: "Electronics",
      user: "JohnDoe"
    },
    { 
      id: 2, 
      title: "Need free car", 
      content: "My car won't move anymore. I think the flywheel disconnected from the torque convertor. Anyone got a low mileage car for free? I'll take a free electric bike too if it has newer batteries but the car will need a rack/hitch for rack. I'll look for a rack in the free section. Thank you for reading.", 
      replies: [
        { user: "Charlie", content: "ebike would be the way to go" },
        { user: "Dave", content: "I have heard a tip to check funerals" }
      ],
      likes: 8,
      label: "Cars",
      user: "JaneSmith"
    }
  ];

  let showPostForm = false;
  let newPostTitle = '';
  let newPostContent = '';
  let selectedPostLabel = '';
  let selectedPost = null;
  let replyContent = '';
  let currentUser = "User123";
  let searchQuery = '';

  const topics = ["Books", "Cars", "Clothes", "Electronics", "Entertainment", "Gaming", "Housing", "Fitness", "Furniture"];

  const addNewPost = () => {
    if (newPostTitle && newPostContent) {
      const newPost = { 
        id: posts.length + 1, 
        title: newPostTitle, 
        content: newPostContent, 
        replies: [], 
        likes: 0, 
        label: selectedPostLabel, 
        user: currentUser 
      };

      posts = [newPost, ...posts];
      
      newPostTitle = '';
      newPostContent = '';
      selectedPostLabel = '';
      showPostForm = false;
    }
  };

  const openPostPopup = (post) => {
    selectedPost = post;
    showPostForm = false; // Hide the "Create New Post" form when viewing a post
  };

  const closePostPopup = () => {
    showPostForm = false;
    selectedPost = null;
  };

  const addReply = () => {
    if (replyContent) {
      selectedPost.replies.push({ user: currentUser, content: replyContent });
      replyContent = ''; 
    }
  };
</script>

<div class="container">
  <!-- Search Bar at the very top -->
  <div class="search-container">
    <input class="search-bar" type="text" bind:value={searchQuery} placeholder="Search posts..." />
    <i class="search-icon">&#128269;</i> <!-- Magnifying glass icon -->
  </div>

  <!-- Main Content (Discussion and Topics) -->
  <div class="main-content">
    <!-- Discussion Area -->
    <div class="discussion">
      <h2>Discussion Area</h2>

      <!-- Button to toggle post creation form -->
      <button class="button" on:click={() => showPostForm = true}>
        Ask a Question
      </button>

      <!-- Display Posts -->
      {#each posts as post}  
        <div class="post" on:click={() => openPostPopup(post)}>
          <div class="post-title">{post.title}</div>
          <div class="post-content">{post.content.slice(0, 100)}...</div>
          <div class="post-meta">
            <span>Likes: {post.likes}</span> | <span>By: {post.user}</span> | <span>Topic: {post.label}</span>
          </div>
        </div>
      {/each}
    </div>

    <!-- Topics Sidebar -->
    <div class="topics">
      <div class="topics-header">
        <h3>Topics</h3>
        <div class="topics-list">
          {#each topics as topic}       
            <div class="topic">{topic}</div>
          {/each}
        </div>
      </div>
    </div>
  </div>

  <!-- Popup for viewing a post -->
  {#if selectedPost}
    <div class="popup-overlay" on:click={closePostPopup}></div>
    <div class="popup">
      <h3>{selectedPost.title}</h3>
      <p>{selectedPost.content}</p>
      <div class="line"></div>

      <div class="replies">
        {#each selectedPost.replies as reply}
          <div class="reply-box">
            <div class="reply-box-user">{reply.user}</div>
            <div class="reply-box-content">{reply.content}</div>
          </div>
        {/each}
      </div>

      <textarea class="reply" bind:value={replyContent} placeholder="Write your reply..."></textarea>
      <button class="button" on:click={addReply}>Reply</button>
      <button class="button" on:click={closePostPopup}>Close</button>
    </div>
  {/if}

  <!-- Popup for creating a post -->
  {#if showPostForm}
    <div class="popup-overlay" on:click={closePostPopup}></div>
    <div class="popup">
      <h3>Create New Post</h3>
      <label for="postTitle">Title:</label>
      <input id="postTitle" bind:value={newPostTitle} class="form-input" type="text" placeholder="Enter title" />
      <label for="postContent">Content:</label>
      <textarea id="postContent" bind:value={newPostContent} class="form-input" placeholder="Write your post..."></textarea>
      <label for="postLabel">Topic:</label>
      <select id="postLabel" bind:value={selectedPostLabel} class="form-input">
        <option value="">Select a topic</option>
        {#each topics as topic}
          <option value={topic}>{topic}</option>
        {/each}
      </select>
      <button class="button" on:click={addNewPost}>Submit Post</button>
      <button class="button" on:click={closePostPopup}>Close</button>
    </div>
  {/if}
</div>

<style>
  .container {
    display: flex;
    flex-direction: column; /* Stack elements vertically */
    height: 100vh;
    padding: 20px;
    background-color: #f4f7f8;
    overflow: hidden;
    max-width: 1200px; /* Optional: Set max-width to constrain the layout */
    margin: 0 auto; /* Center the container */
  }
  
  .search-container {
    width: 100%;
    padding-bottom: 20px;
    position: sticky;
    top: 0;
    z-index: 0;
  }
  
  .search-bar {
    margin: 0;
    padding: 12px;
    font-size: 16px;
    width: 100%;
    border-radius: 5px;
    border: 1px solid #ddd;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding-left: 35px;
    position: relative;
  }
  
  .search-icon {
    position: absolute;
    left: 10px;
    top: 50%;
    transform: translateY(-50%);
    color: #777;
    font-size: 18px;
  }
  
  .main-content {
    display: flex;
    flex: 1;
    margin-top: 20px;
    width: 100%; /* Ensure the content takes the full width */
  }
  
  .discussion {
    flex: 3;
    padding: 20px;
    margin-right: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .topics {
    flex: 1;
    width: 25%;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .topic {
    margin-bottom: 10px;
    cursor: pointer;
    padding: 8px;
    background-color: #f4f4f4;
    border-radius: 5px;
    transition: background-color 0.3s ease;
  }
  
  .topic:hover {
    background-color: #e0e0e0;
  }
  
  .post {
    margin-bottom: 20px;
    cursor: pointer;
    padding: 15px;
    background-color: #ffffff;
    border-radius: 8px;
    border: 1px solid #ddd;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .post:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 12px rgba(0, 0, 0, 0.2);
  }
  
  .post-title {
    font-weight: bold;
    font-size: 18px;
  }
  
  .post-content {
    margin-top: 5px;
    color: #555;
    font-size: 14px;
  }
  
  .post-meta {
    margin-top: 5px;
    font-size: 12px;
    color: #777;
  }
  
  .button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    margin-bottom: 20px;
    background-color: #781097;
    color: white;
    border: none;
    border-radius: 5px;
  }
  
  .form-input {
    margin-bottom: 10px;
    padding: 8px;
    width: 100%;
    font-size: 14px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  .popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    border: 1px solid #ccc;
    padding: 20px;
    width: 60%;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    z-index:1000;
  }
  
  .popup-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 1000;
  }
  
  .reply-box {
    margin-top: 10px;
    padding: 10px;
    background-color: #f1f1f1;
    border-radius: 5px;
    margin-bottom: 10px;
  }
  
  .reply-box-user {
    font-weight: bold;
  }
  
  .reply-box-content {
    margin-top: 5px;
    color: #555;
  }
  
  .reply {
    margin-top: 10px;
    padding: 8px;
    font-size: 14px;
    width: 100%;
  }
  
  .replies {
    margin-top: 20px;
  }
  
  .line {
    border-top: 1px solid #ccc;
    margin: 10px 0;
  }
  
  .topics-header {
    display: flex;
    flex-direction: column;
  }
  
  .topics-list {
    margin-top: 20px;
  }
  
  .topics-header button {
    margin-bottom: 20px;
  }
  
  .topics-list .topic {
    cursor: pointer;
  }
  
  @media screen and (max-width: 768px) {
    .main-content {
      flex-direction: column;
    }
  
    .topics {
      width: 100%;
      margin-top: 20px;
    }
  }
  
</style>
