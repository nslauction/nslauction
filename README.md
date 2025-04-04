<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Player Auction Dashboard</title>
  <!-- Google Font for a modern, sporty look -->
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
  <style>
    /* FIFA 23 Inspired Theme */
    body {
      background: linear-gradient(135deg, #0f0f0f, #3b3b3b);
      font-family: 'Orbitron', sans-serif;
      margin: 0;
      padding: 20px;
      min-height: 100vh;
      color: #eaeaea;
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    h1 {
      font-size: 2.5rem;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.6);
      margin-bottom: 20px;
      color: #FFD700;
    }
    .user-container {
      width: 340px;
      padding: 20px;
      background: #1c1c1c;
      color: #eaeaea;
      border-radius: 10px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5);
      border: 2px solid #FFD700;
      margin-bottom: 20px;
    }
    .user-container input, .user-container select, .user-container button {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      box-sizing: border-box;
      border: none;
      border-radius: 5px;
      font-family: inherit;
      font-size: 1rem;
    }
    .user-container button {
      background: #000;
      border: 2px solid #FFD700;
      color: #FFD700;
      transition: background 0.3s ease, transform 0.2s ease;
      cursor: pointer;
    }
    .user-container button:hover {
      background: #333;
      transform: scale(1.02);
    }
    .flex-container {
      display: flex;
      gap: 20px;
      width: 100%;
      justify-content: center;
      flex-wrap: wrap;
    }
    .container, .list-container {
      width: 340px;
      padding: 20px;
      background: #1c1c1c;
      color: #eaeaea;
      border-radius: 10px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5);
      border: 2px solid #FFD700;
    }
    textarea, input {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      box-sizing: border-box;
      border: none;
      border-radius: 5px;
      font-family: inherit;
      font-size: 1rem;
    }
    button {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      cursor: pointer;
      background: #000;
      border: 2px solid #FFD700;
      color: #FFD700;
      font-size: 1rem;
      border-radius: 5px;
      transition: background 0.3s ease, transform 0.2s ease;
      font-family: inherit;
    }
    button:hover {
      background: #333;
      transform: scale(1.02);
    }
    .timer {
      font-size: 1.8rem;
      font-weight: bold;
      margin-bottom: 10px;
      color: #FFD700;
    }
    .progress-bar-container {
      width: 100%;
      background-color: #555;
      height: 20px;
      border-radius: 10px;
      overflow: hidden;
      margin-bottom: 10px;
    }
    .progress-bar {
      height: 100%;
      background-color: #FFD700;
      width: 0%;
      transition: width 1s linear;
    }
    .auction-box {
      height: 150px;
      overflow-y: auto;
      border: 2px solid #FFD700;
      padding: 10px;
      background: #000;
      margin-bottom: 10px;
      color: #FFD700;
      font-size: 0.9rem;
      text-align: left;
    }
    .selected-player {
      font-size: 1.8rem;
      font-weight: bold;
      margin: 15px 0;
      color: #FFD700;
      background: #000;
      padding: 10px;
      border-radius: 5px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
      transition: transform 0.3s ease, background 0.3s ease;
    }
    .selected-player.active {
      transform: scale(1.05);
      background: #111;
    }
    li {
      margin: 5px 0;
      padding: 5px;
      background: #FFD700;
      border-radius: 3px;
      color: #000;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
    }
    .feedback-message {
      margin-top: 10px;
      padding: 10px;
      background: #FFD700;
      color: #000;
      border-radius: 5px;
      display: none;
      font-size: 0.9rem;
    }
  </style>
</head>
<body>
  <h1>Player Auction Dashboard</h1>

  <!-- User Management Section -->
  <div class="user-container">
    <h2>Select User</h2>
    <select id="userSelect" onchange="changeUser()"></select>
    <h3>Add New User</h3>
    <input type="text" id="newUsername" placeholder="Enter username" />
    <input type="number" id="newBudget" placeholder="Enter starting budget" min="1" />
    <button onclick="addUser()">Add User</button>
  </div>

  <div class="timer" id="timer">Time Left: --</div>
  <div class="progress-bar-container">
    <div class="progress-bar" id="progressBar"></div>
  </div>
  <div class="flex-container">
    <div class="container">
      <h2>Total Budget: <span id="totalBudget">1500</span>/-</h2>
    </div>
    <div class="container">
      <textarea id="playerInput" placeholder="Paste player names here (one per line)"></textarea>
      <button onclick="updatePlayerList()">Update List</button>
      <button onclick="chooseRandomPlayer()">Choose Random Player</button>
      <div class="selected-player" id="selectedPlayer">No player selected</div>
    </div>
  </div>
  <div class="container">
    <h2>Current Bidding</h2>
    <div class="auction-box" id="auctionBox"></div>
    <input type="number" id="bidInput" placeholder="Enter your bid..." min="1">
    <button onclick="placeBid()">Bid</button>
    <div class="feedback-message" id="feedbackMessage"></div>
  </div>
  <div class="flex-container">
    <div class="list-container">
      <h2>All Players</h2>
      <ul id="playerList"></ul>
    </div>
    <div class="list-container">
      <h2>Sold Players</h2>
      <ul id="soldList"></ul>
    </div>
    <div class="list-container">
      <h2>Unsold Players</h2>
      <ul id="unsoldList"></ul>
    </div>
  </div>

  <!-- Audio elements for sound effects -->
  <audio id="siuuSound" src="https://www.myinstants.com/media/sounds/siuuu.mp3" preload="auto"></audio>
  <audio id="queMiraBoboSound" src="https://www.myinstants.com/media/sounds/que-mira-bobo-messi.mp3" preload="auto"></audio>

  <script>
    // Global variables for the current user and auction state
    let currentUser = null;
    let username = "";
    let totalBudget = 1500;
    let players = [];
    let currentPlayer = null;
    let highestBid = 0;
    let highestBidder = null;
    let timeLeft = 0;
    let totalTime = 30;
    let bidPlaced = false;
    let timer = null;

    // Load users from localStorage
    let users = JSON.parse(localStorage.getItem("users")) || [];
    populateUserSelect();

    // If there are any users, select the first one automatically
    if (users.length > 0) {
      currentUser = users[0];
      username = currentUser.username;
      totalBudget = currentUser.budget;
    }
    document.getElementById("totalBudget").textContent = totalBudget;
    loadSavedLists();
    loadAuctionHistory();

    // Populate the user select dropdown
    function populateUserSelect() {
      const userSelect = document.getElementById("userSelect");
      userSelect.innerHTML = "";
      users.forEach((user, index) => {
        const option = document.createElement("option");
        option.value = index;
        option.textContent = user.username + " (Budget: " + user.budget + ")";
        userSelect.appendChild(option);
      });
    }

    // Change current user when selection changes
    function changeUser() {
      const userSelect = document.getElementById("userSelect");
      const selectedIndex = userSelect.value;
      currentUser = users[selectedIndex];
      username = currentUser.username;
      totalBudget = currentUser.budget;
      document.getElementById("totalBudget").textContent = totalBudget;
      // Load user-specific data from localStorage
      players = JSON.parse(localStorage.getItem(`${username}_players`)) || [];
      loadSavedLists();
      loadAuctionHistory();
    }

    // Add a new user with a starting budget
    function addUser() {
      const newUsername = document.getElementById("newUsername").value.trim();
      const newBudget = parseInt(document.getElementById("newBudget").value.trim());
      if (!newUsername || isNaN(newBudget) || newBudget <= 0) {
        showFeedback("Please enter a valid username and budget.");
        return;
      }
      // Check if user already exists
      if (users.some(user => user.username === newUsername)) {
        showFeedback("Username already exists!");
        return;
      }
      const newUser = { username: newUsername, budget: newBudget };
      users.push(newUser);
      localStorage.setItem("users", JSON.stringify(users));
      populateUserSelect();
      // Optionally, select the new user immediately
      document.getElementById("userSelect").value = users.length - 1;
      changeUser();
      document.getElementById("newUsername").value = "";
      document.getElementById("newBudget").value = "";
      showFeedback(`User ${newUsername} added successfully!`);
    }

    function startTimer() {
      clearInterval(timer);
      timeLeft = 30;
      totalTime = 30;
      bidPlaced = false;
      updateTimer();
      timer = setInterval(() => {
        timeLeft--;
        updateTimer();
        if (timeLeft <= 20 && !bidPlaced && currentPlayer) {
          clearInterval(timer);
          markUnsoldAutomatically();
        } else if (timeLeft <= 0) {
          clearInterval(timer);
          endAuction();
        }
      }, 1000);
    }

    function updateTimer() {
      document.getElementById("timer").textContent = currentPlayer ? `Time Left: ${timeLeft}s` : "Time Left: --";
      const progressBar = document.getElementById("progressBar");
      progressBar.style.width = currentPlayer ? `${(timeLeft / totalTime) * 100}%` : "0%";
      const selectedPlayer = document.getElementById("selectedPlayer");
      if (currentPlayer) {
        selectedPlayer.classList.add("active");
      } else {
        selectedPlayer.classList.remove("active");
      }
    }

    function placeBid() {
      if (!currentPlayer) {
        showFeedback("Please select a player first!");
        return;
      }
      const bidInput = document.getElementById("bidInput");
      const bid = parseInt(bidInput.value.trim());
      if (isNaN(bid) || bid <= highestBid || bid > totalBudget) {
        showFeedback(`Invalid bid! Must be higher than ${highestBid} and within your budget (${totalBudget}).`);
        return;
      }
      highestBid = bid;
      highestBidder = username;
      bidPlaced = true;
      const bidMsg = `${username} bids: ${bid} on ${currentPlayer}`;
      const div = document.createElement("div");
      div.textContent = bidMsg;
      document.getElementById("auctionBox").appendChild(div);
      document.getElementById("auctionBox").scrollTop = document.getElementById("auctionBox").scrollHeight;
      bidInput.value = "";
      showFeedback("Bid placed successfully!");
      saveAuctionHistory(bidMsg);
    }

    function updatePlayerList() {
      const input = document.getElementById("playerInput").value.trim();
      if (!input) {
        showFeedback("Please enter at least one player!");
        return;
      }
      players = input.split('\n').map(player => player.trim()).filter(player => player);
      if (new Set(players).size !== players.length) {
        showFeedback("Player names must be unique!");
        return;
      }
      localStorage.setItem(`${username}_players`, JSON.stringify(players));
      document.getElementById("playerInput").value = '';
      showFeedback("Player list updated successfully!");
      updateUI();
    }

    function chooseRandomPlayer() {
      if (players.length === 0) {
        showFeedback("Please add players first!");
        return;
      }
      const availablePlayers = players.filter(player => {
        return ![...document.getElementById("soldList").children, 
                ...document.getElementById("unsoldList").children]
          .some(li => li.dataset.player === player);
      });
      if (availablePlayers.length === 0) {
        showFeedback("No available players left!");
        return;
      }
      currentPlayer = availablePlayers[Math.floor(Math.random() * availablePlayers.length)];
      highestBid = 0;
      highestBidder = null;
      document.getElementById("selectedPlayer").textContent = currentPlayer;
      document.getElementById("auctionBox").innerHTML = '';
      showFeedback(`Player ${currentPlayer} selected for auction!`);
      startTimer();
      updateUI();
    }

    function endAuction() {
      if (currentPlayer && highestBidder && highestBid > 0) {
        totalBudget -= highestBid;
        // Update current user's budget
        currentUser.budget = totalBudget;
        localStorage.setItem("users", JSON.stringify(users));
        document.getElementById("totalBudget").textContent = totalBudget;
        localStorage.setItem(`${username}_budget`, totalBudget);
        const playerItems = document.getElementById("playerList").getElementsByTagName("li");
        for (let item of playerItems) {
          if (item.dataset.player === currentPlayer) {
            item.querySelector("input[type='number']").value = highestBid;
            item.querySelector("input[type='text']").value = highestBidder;
            markSold(item);
            break;
          }
        }
        showFeedback(`${currentPlayer} sold to ${highestBidder} for ${highestBid}!`);
        // Play "Siuu" sound effect with error handling
        const siuuSound = document.getElementById("siuuSound");
        siuuSound.currentTime = 0;
        siuuSound.play().catch(err => console.log("Siuu sound failed to play:", err));
      }
      resetAuction();
    }

    function markUnsoldAutomatically() {
      if (currentPlayer) {
        const playerItems = document.getElementById("playerList").getElementsByTagName("li");
        for (let item of playerItems) {
          if (item.dataset.player === currentPlayer) {
            markUnsold(item);
            break;
          }
        }
        showFeedback(`${currentPlayer} went unsold!`);
        // Play "Que Mira Bobo" sound effect with error handling
        const queMiraBoboSound = document.getElementById("queMiraBoboSound");
        queMiraBoboSound.currentTime = 0;
        queMiraBoboSound.play().catch(err => console.log("Que Mira Bobo sound failed to play:", err));
      }
      resetAuction();
    }

    function resetAuction() {
      currentPlayer = null;
      highestBid = 0;
      highestBidder = null;
      bidPlaced = false;
      document.getElementById("selectedPlayer").textContent = "No player selected";
      updateTimer();
      updateUI();
    }

    function markSold(playerItem) {
      document.getElementById("soldList").appendChild(playerItem);
      saveLists();
    }

    function markUnsold(playerItem) {
      document.getElementById("unsoldList").appendChild(playerItem);
      saveLists();
    }

    function updateUI() {
      const playerList = document.getElementById("playerList");
      playerList.innerHTML = '';
      players.forEach(player => {
        if (![...document.getElementById("soldList").children, 
               ...document.getElementById("unsoldList").children]
            .some(li => li.dataset.player === player)) {
          const li = document.createElement('li');
          li.innerHTML = `${player} - <input type="number" placeholder="Price" min="1" disabled> <input type="text" placeholder="Owner" disabled>`;
          li.dataset.player = player;
          playerList.appendChild(li);
        }
      });
      saveLists();
    }

    function saveLists() {
      const allPlayers = Array.from(document.getElementById("playerList").children).map(li => ({
        name: li.dataset.player,
        price: li.querySelector("input[type='number']").value,
        owner: li.querySelector("input[type='text']").value
      }));
      const soldPlayers = Array.from(document.getElementById("soldList").children).map(li => ({
        name: li.dataset.player,
        price: li.querySelector("input[type='number']").value,
        owner: li.querySelector("input[type='text']").value
      }));
      const unsoldPlayers = Array.from(document.getElementById("unsoldList").children).map(li => ({
        name: li.dataset.player,
        price: li.querySelector("input[type='number']").value,
        owner: li.querySelector("input[type='text']").value
      }));

      localStorage.setItem(`${username}_allPlayers`, JSON.stringify(allPlayers));
      localStorage.setItem(`${username}_soldPlayers`, JSON.stringify(soldPlayers));
      localStorage.setItem(`${username}_unsoldPlayers`, JSON.stringify(unsoldPlayers));
    }

    function loadSavedLists() {
      const allPlayers = JSON.parse(localStorage.getItem(`${username}_allPlayers`)) || [];
      const soldPlayers = JSON.parse(localStorage.getItem(`${username}_soldPlayers`)) || [];
      const unsoldPlayers = JSON.parse(localStorage.getItem(`${username}_unsoldPlayers`)) || [];

      const playerList = document.getElementById("playerList");
      const soldList = document.getElementById("soldList");
      const unsoldList = document.getElementById("unsoldList");

      allPlayers.forEach(player => {
        const li = document.createElement('li');
        li.innerHTML = `${player.name} - <input type="number" placeholder="Price" min="1" disabled value="${player.price || ''}"> <input type="text" placeholder="Owner" disabled value="${player.owner || ''}">`;
        li.dataset.player = player.name;
        playerList.appendChild(li);
      });

      soldPlayers.forEach(player => {
        const li = document.createElement('li');
        li.innerHTML = `${player.name} - <input type="number" placeholder="Price" min="1" disabled value="${player.price || ''}"> <input type="text" placeholder="Owner" disabled value="${player.owner || ''}">`;
        li.dataset.player = player.name;
        soldList.appendChild(li);
      });

      unsoldPlayers.forEach(player => {
        const li = document.createElement('li');
        li.innerHTML = `${player.name} - <input type="number" placeholder="Price" min="1" disabled value="${player.price || ''}"> <input type="text" placeholder="Owner" disabled value="${player.owner || ''}">`;
        li.dataset.player = player.name;
        unsoldList.appendChild(li);
      });
    }

    function saveAuctionHistory(bidMsg) {
      let history = JSON.parse(localStorage.getItem(`${username}_auctionHistory`)) || [];
      history.push(bidMsg);
      localStorage.setItem(`${username}_auctionHistory`, JSON.stringify(history));
      loadAuctionHistory();
    }

    function loadAuctionHistory() {
      const history = JSON.parse(localStorage.getItem(`${username}_auctionHistory`)) || [];
      const auctionBox = document.getElementById("auctionBox");
      auctionBox.innerHTML = '';
      history.forEach(msg => {
        const div = document.createElement("div");
        div.textContent = msg;
        auctionBox.appendChild(div);
      });
      auctionBox.scrollTop = auctionBox.scrollHeight;
    }

    function showFeedback(message) {
      const feedback = document.getElementById("feedbackMessage");
      feedback.textContent = message;
      feedback.style.display = "block";
      setTimeout(() => {
        feedback.style.display = "none";
      }, 3000);
    }
  </script>
</body>
</html>

