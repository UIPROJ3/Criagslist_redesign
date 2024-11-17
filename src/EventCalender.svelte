<script>
  import { onMount } from 'svelte';

  // Sample events data
  let events = [
    { title: 'Music Concert', date: '2024-11-16', description: 'A fun music concert featuring top artists.', image: '/assets/music-concert.jpg' },
    { title: 'Tech Conference', date: '2024-11-18', description: 'Join the latest in tech innovation talks.', image: '/assets/tech-conference.jpg' },
    { title: 'Art Exhibition', date: '2024-11-20', description: 'An art exhibition showcasing modern art.', image: '/assets/art-exhibition.jpg' },
  ];

  let selectedEvent = null;
  let isModalOpen = false;
  let selectedDate = new Date().toISOString().split('T')[0]; // Default to today

  const openEventDetails = (event) => {
    selectedEvent = event;
    isModalOpen = true;
  };

  const closeModal = () => {
    isModalOpen = false;
    selectedEvent = null;
  };

  const getEventsByDate = (date) => {
    return events.filter((event) => event.date === date);
  };

  let currentMonth = new Date().getMonth();
  let currentYear = new Date().getFullYear();

  // Calculate the days in the current month
  const daysInMonth = (month, year) => {
    let date = new Date(year, month, 0);
    return date.getDate();
  };

  let days = Array.from({ length: daysInMonth(currentMonth + 1, currentYear) }, (_, i) => i + 1);

  // Function to handle next/previous month navigation
  const changeMonth = (direction) => {
    if (direction === 'next') {
      currentMonth++;
      if (currentMonth > 11) {
        currentMonth = 0;
        currentYear++;
      }
    } else {
      currentMonth--;
      if (currentMonth < 0) {
        currentMonth = 11;
        currentYear--;
      }
    }
    days = Array.from({ length: daysInMonth(currentMonth + 1, currentYear) }, (_, i) => i + 1);
  };
</script>

<style>
  .calendar-container {
    overflow-x: auto;
    white-space: nowrap;
    margin-bottom: 20px;
    padding: 10px;
    background-color: #f4f7f9;
    border-radius: 8px;
  }

  .calendar-day {
    display: inline-block;
    padding: 20px;
    text-align: center;
    border: 2px solid transparent;
    border-radius: 8px;
    cursor: pointer;
    margin: 0 10px;
    transition: all 0.3s ease;
    background-color: #fff;
  }

  .calendar-day:hover {
    background-color: #4CAF50;
    color: white;
    border-color: #388E3C;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }

  .calendar-day.selected {
    background-color: #388E3C;
    color: white;
    border-color: #4CAF50;
  }

  .calendar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }

  .calendar-header h3 {
    margin: 0;
    font-size: 24px;
    font-weight: 500;
    color: #333;
  }

  .calendar-header button {
    background-color: #00796b;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .calendar-header button:hover {
    background-color: #004d40;
  }

  .event-list {
    margin-top: 20px;
  }

  .event-card {
    display: flex;
    align-items: center;
    background-color: #fff;
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 15px;
    cursor: pointer;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .event-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }

  .event-card img {
    width: 60px;
    height: 60px;
    border-radius: 5px;
    margin-right: 20px;
  }

  .event-info {
    flex: 1;
  }

  .event-info h3 {
    margin: 0;
    font-size: 18px;
    color: #333;
  }

  .event-info p {
    margin: 5px 0 0;
    font-size: 14px;
    color: #777;
  }

  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    z-index: 1000;
    width: 400px;
    max-width: 100%;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s, visibility 0.3s;
  }

  .modal.show {
    opacity: 1;
    visibility: visible;
  }

  .modal-content img {
    width: 100%;
    border-radius: 5px;
  }

  .modal-content h3 {
    font-size: 20px;
    color: #333;
  }

  .modal-content p {
    color: #555;
  }

  .event-tooltip {
    position: absolute;
    background-color: #333;
    color: white;
    padding: 5px;
    font-size: 12px;
    border-radius: 3px;
    top: -30px;
  }
</style>

<div>
  <div class="calendar-header">
    <button on:click={() => changeMonth('prev')}>←</button>
    <h3>{new Date(currentYear, currentMonth).toLocaleString('default', { month: 'long' })} {currentYear}</h3>
    <button on:click={() => changeMonth('next')}>→</button>
  </div>

  <div class="calendar-container">
    {#each days as day}
      <div
        class="calendar-day {selectedDate === `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}` ? 'selected' : ''}"
        on:click={() => { selectedDate = `${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`; }}
      >
        {day}
        {#if getEventsByDate(`${currentYear}-${String(currentMonth + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`).length > 0}
          <div class="event-tooltip">Events</div>
        {/if}
      </div>
    {/each}
  </div>

  <h2>Events for {selectedDate}</h2>
  <div class="event-list">
    {#each getEventsByDate(selectedDate) as event}
      <div class="event-card" on:click={() => openEventDetails(event)}>
        <img src={event.image} alt={event.title} class="event-image" />
        <div class="event-info">
          <h3>{event.title}</h3>
          <p>{event.date}</p>
        </div>
      </div>
    {/each}
    {#if getEventsByDate(selectedDate).length === 0}
      <p>No events scheduled for this date.</p>
    {/if}
  </div>

  {#if isModalOpen}
    <div class="modal show">
      <div class="modal-content">
        <h3>{selectedEvent.title}</h3>
        <img src={selectedEvent.image} alt={selectedEvent.title} />
        <p>{selectedEvent.description}</p>
        <button on:click={closeModal}>Close</button>
      </div>
    </div>
  {/if}
</div>
