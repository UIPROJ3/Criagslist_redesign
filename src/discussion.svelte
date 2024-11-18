<script>
  import "./discussion.css"
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
