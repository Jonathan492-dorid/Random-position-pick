<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Random Position Picker (Firestore + Modular SDK)</title>
<style>
  body {
    background-color: #000;
    font-family: Arial, sans-serif;
    color: #0ff;
    text-align: center;
    padding: 20px;
  }
  h1 {
    color: #0ff;
    text-shadow: 0 0 10px #0ff;
  }
  .game-board {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 15px;
    max-width: 480px;
    margin: auto;
  }
  .box {
    background-color: #111;
    border: 2px solid #0ff;
    padding: 20px;
    font-size: 1.5em;
    color: #0ff;
    cursor: pointer;
    border-radius: 10px;
    transition: 0.3s;
    text-shadow: 0 0 5px #0ff;
    user-select: none;
  }
  .box.disabled {
    background-color: #333;
    color: #555;
    cursor: not-allowed;
    border-color: #555;
  }
  .box.user-choice {
    border-color: #ff0;
    background-color: #222;
    color: #ff0;
    font-weight: bold;
  }
  .box:hover:not(.disabled):not(.user-choice) {
    background-color: #0ff;
    color: #000;
  }
  #chosen {
    margin-top: 20px;
    font-size: 1.2em;
  }
  button {
    padding: 10px 15px;
    margin-top: 20px;
    font-size: 1em;
    border-radius: 5px;
    background-color: #0ff;
    color: #000;
    border: none;
    cursor: pointer;
    font-weight: bold;
  }
  button:hover {
    background-color: #00cccc;
  }
</style>
</head>
<body>
<h1>🎲 Random Position Picker (Firestore) 🎲</h1>
<p>Click a box to pick your random position. One choice per user.</p>

<div class="game-board" id="board"></div>

<div id="chosen"><strong>Your Chosen Number:</strong> None yet</div>

<button id="resetBtn">🔄 Reset Game (Admin Only)</button>

<!-- Firebase SDK (modular) -->
<script type="module">
  import { initializeApp } from 'https://www.gstatic.com/firebasejs/9.22.1/firebase-app.js';
  import {
    getFirestore, collection, doc, getDocs,
    setDoc, onSnapshot, deleteDoc
  } from 'https://www.gstatic.com/firebasejs/9.22.1/firebase-firestore.js';

  // Your Firebase config — replace with your actual config!
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
  };

  const ADMIN_PASSWORD = "20";

  // Initialize Firebase app and Firestore
  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);

  const board = document.getElementById('board');
  const chosenDiv = document.getElementById('chosen');
  const resetBtn = document.getElementById('resetBtn');

  // Generate or get unique userId locally
  let userId = localStorage.getItem('userId');
  if (!userId) {
    userId = 'user_' + Math.random().toString(36).substring(2, 11);
    localStorage.setItem('userId', userId);
  }

  // Admin mode flag and prompt
  let isAdmin = false;
  window.onload = () => {
    const pw = prompt("Enter admin password to enable notifications (or leave blank):");
    if (pw === ADMIN_PASSWORD) {
      isAdmin = true;
      alert("Admin mode enabled. You will get notifications when users pick.");
    }
  };

  const choicesCol = collection(db, 'choices');
  let userChoice = null; // { boxNum, number }

  // Listen for real-time updates from Firestore choices collection
  let previousChoices = {};

  onSnapshot(choicesCol, (snapshot) => {
    const currentChoices = {};
    snapshot.forEach(docSnap => {
      currentChoices[docSnap.id] = docSnap.data();
    });

    // Update userChoice if saved on server
    if (currentChoices[userId]) {
      userChoice = currentChoices[userId];
      localStorage.setItem('userChoice', JSON.stringify(userChoice));
    } else {
      // Try fallback from localStorage
      const localChoice = localStorage.getItem('userChoice');
      if (localChoice) {
        try {
          userChoice = JSON.parse(localChoice);
        } catch {
          userChoice = null;
        }
      } else {
        userChoice = null;
      }
    }

    // Notify admin about new picks
    if (isAdmin) {
      for (const [uid, choice] of Object.entries(currentChoices)) {
        if (!previousChoices[uid]) {
          alert(`User ${uid} picked Box ${choice.boxNum} with position ${choice.number}`);
        }
      }
    }
    previousChoices = currentChoices;

    renderBoard(currentChoices);
    updateChosenList();
  });

  function renderBoard(choices) {
    board.innerHTML = '';
    const takenNumbers = new Set(Object.values(choices).map(c => c.number));
    const takenBoxes = new Set(Object.values(choices).map(c => c.boxNum));

    for (let i = 1; i <= 12; i++) {
      const box = document.createElement('div');
      box.classList.add('box');
      box.dataset.boxNum = i;

      if (userChoice && userChoice.boxNum === i) {
        box.textContent = userChoice.number;
        box.classList.add('disabled', 'user-choice');
      } else if (takenBoxes.has(i)) {
        box.textContent = `Box ${i}`;
        box.classList.add('disabled');
      } else {
        box.textContent = `Box ${i}`;
        if (!userChoice) {
          box.onclick = () => chooseBox(i, choices);
        } else {
          box.classList.add('disabled');
        }
      }
      board.appendChild(box);
    }
  }

  async function chooseBox(boxNum, choices) {
    if (userChoice) {
      alert("❌ You have already chosen your box!");
      return;
    }

    const takenNumbers = new Set(Object.values(choices).map(c => c.number));
    const takenBoxes = new Set(Object.values(choices).map(c => c.boxNum));

    if (takenBoxes.has(boxNum)) {
      alert("❌ This box is already taken by another user.");
      return;
    }

    // Find available numbers 1-12 that are not taken
    const availableNumbers = [];
    for (let n = 1; n <= 12; n++) {
      if (!takenNumbers.has(n)) availableNumbers.push(n);
    }

    if (availableNumbers.length === 0) {
      alert("❌ No available numbers left!");
      return;
    }

    // Random pick from available numbers
    const randomIndex = Math.floor(Math.random() * availableNumbers.length);
    const assignedNumber = availableNumbers[randomIndex];

    const newChoice = { boxNum, number: assignedNumber };

    try {
      await setDoc(doc(db, 'choices', userId), newChoice);
      userChoice = newChoice;
      localStorage.setItem('userChoice', JSON.stringify(userChoice));
      renderBoard(await getAllChoices());
      updateChosenList();
    } catch (e) {
      alert("Error saving your choice. Please try again.");
      console.error(e);
    }
  }

  async function getAllChoices() {
    const snapshot = await getDocs(choicesCol);
    const allChoices = {};
    snapshot.forEach(docSnap => {
      allChoices[docSnap.id] = docSnap.data();
    });
    return allChoices;
  }

  function updateChosenList() {
    if (!userChoice) {
      chosenDiv.innerHTML = "<strong>Your Chosen Number:</strong> None yet";
    } else {
      chosenDiv.innerHTML = `<strong>Your Chosen Number:</strong> ${userChoice.number} (Box ${userChoice.boxNum})`;
    }
  }

  async function resetGame() {
    if (!isAdmin) {
      alert("Only admin can reset the game.");
      return;
    }
    try {
      const snapshot = await getDocs(choicesCol);
      const batchDeletes = snapshot.docs.map(docSnap => deleteDoc(doc(db, 'choices', docSnap.id)));
      await Promise.all(batchDeletes);
      localStorage.removeItem('userChoice');
      userChoice = null;
      renderBoard({});
      updateChosenList();
      alert("Game reset successfully!");
    } catch (e) {
      alert("Error resetting game.");
      console.error(e);
    }
  }

  resetBtn.addEventListener('click', async () => {
    const inputPassword = prompt("Enter admin password to reset:");
    if (inputPassword === ADMIN_PASSWORD) {
      isAdmin = true; // Enable admin mode if password correct
      await resetGame();
    } else {
      alert("❌ Incorrect password! Reset not allowed.");
    }
  });
</script>
</body>
</html>
