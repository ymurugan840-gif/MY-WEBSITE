# MY-WEBSITE<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SmartStudy AI</title>

  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">

  <!-- Material Icons -->
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

  <style>
    :root {
      --primary: #1a73e8;
      --background: #ffffff;
      --surface: #ffffff;
      --text: #202124;
      --soft-text: #5f6368;
      --card: #ffffff;
    }

    body.dark {
      --primary: #8ab4f8;
      --background: #121212;
      --surface: #1e1e1e;
      --text: #e8eaed;
      --soft-text: #9aa0a6;
      --card: #1f1f1f;
    }

    body {
      margin: 0;
      font-family: "Roboto", sans-serif;
      background: var(--background);
      color: var(--text);
      transition: 0.4s;
    }

    /* LOADING SCREEN */
    #loader {
      position: fixed;
      background: var(--background);
      width: 100%;
      height: 100%;
      z-index: 9999;
      display: flex;
      justify-content: center;
      align-items: center;
      animation: fadeOut 1s ease 2s forwards;
    }

    @keyframes fadeOut {
      to { opacity: 0; visibility: hidden; }
    }

    .loader-circle {
      width: 48px;
      height: 48px;
      border: 5px solid var(--primary);
      border-top: 5px solid transparent;
      border-radius: 50%;
      animation: spin 1s linear infinite;
    }

    @keyframes spin {
      to { transform: rotate(360deg); }
    }

    /* HEADER */
    header {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 18px 20px;
      background: var(--primary);
      color: white;
      font-size: 24px;
      font-weight: 500;
    }

    /* SIDEBAR */
    #sidebar {
      position: fixed;
      top: 0;
      left: -260px;
      width: 260px;
      height: 100%;
      background: var(--surface);
      padding: 20px;
      box-shadow: 2px 0 10px rgba(0,0,0,0.2);
      transition: 0.3s;
      z-index: 2000;
    }

    #sidebar.open {
      left: 0;
    }

    .menu-item {
      padding: 12px 10px;
      margin-bottom: 10px;
      border-radius: 8px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 10px;
      transition: 0.2s;
    }

    .menu-item:hover {
      background: #e8f0fe;
    }

    /* MAIN CONTAINER */
    .container {
      max-width: 850px;
      margin: 40px auto;
      background: var(--card);
      padding: 25px;
      border-radius: 12px;
      box-shadow: 0px 4px 15px rgba(0,0,0,0.1);
    }

    textarea, input, select {
      width: 100%;
      padding: 12px;
      font-size: 16px;
      border: 1px solid #dadce0;
      border-radius: 8px;
      background: var(--surface);
      color: var(--text);
    }

    button {
      padding: 14px;
      width: 100%;
      font-size: 17px;
      background: var(--primary);
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      opacity: 0.9;
    }

    /* OUTPUT */
    .output-box {
      margin-top: 25px;
      padding: 20px;
      background: #e8f0fe;
      border-left: 5px solid var(--primary);
      border-radius: 8px;
      white-space: pre-line;
    }

    /* AI CHATBOT BUTTON */
    #chat-btn {
      position: fixed;
      bottom: 25px;
      right: 25px;
      background: var(--primary);
      color: white;
      padding: 18px;
      border-radius: 50%;
      font-size: 30px;
      cursor: pointer;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
      z-index: 3000;
    }

    /* CHAT WINDOW */
    #chat-window {
      position: fixed;
      bottom: 90px;
      right: 25px;
      width: 300px;
      background: var(--surface);
      border-radius: 12px;
      box-shadow: 0 5px 20px rgba(0,0,0,0.3);
      padding: 15px;
      display: none;
      flex-direction: column;
      gap: 10px;
    }

    #chat-window textarea {
      height: 80px;
    }

  </style>
</head>

<body>

<!-- LOADING SCREEN -->
<div id="loader">
  <div class="loader-circle"></div>
</div>

<!-- HEADER -->
<header>
  <span class="material-icons" onclick="toggleSidebar()" style="cursor: pointer;">menu</span>
  <span class="material-icons">auto_stories</span>
  SmartStudy AI
</header>

<!-- SIDEBAR MENU -->
<div id="sidebar">
  <div class="menu-item" onclick="toggleDarkMode()">
    <span class="material-icons">dark_mode</span> Toggle Dark Mode
  </div>
  <div class="menu-item" onclick="downloadPDF()">
    <span class="material-icons">picture_as_pdf</span> Download Plan as PDF
  </div>
  <div class="menu-item">
    <span class="material-icons">help_outline</span> Help
  </div>
</div>

<!-- MAIN CONTAINER -->
<div class="container">
  <h2>Create Your Personalized Study Plan</h2>

  <label>Enter Your Subjects:</label>
  <textarea id="subjects" placeholder="Example: Math, Physics, English"></textarea>

  <button onclick="startVoiceInput()">
    🎤 Speak Subjects
  </button>

  <br><br>

  <label>Hours Available Per Day:</label>
  <input id="hours" type="number" placeholder="Example: 4">

  <label>Difficulty Level:</label>
  <select id="difficulty">
    <option value="easy">Easy</option>
    <option value="medium">Medium</option>
    <option value="hard">Hard</option>
  </select>

  <button onclick="generatePlan()">Generate Study Plan</button>

  <div class="output-box" id="output">Your plan will appear here...</div>
</div>

<!-- AI CHAT BUTTON -->
<div id="chat-btn" onclick="toggleChat()">💬</div>

<!-- CHAT WINDOW -->
<div id="chat-window">
  <textarea id="chat-input" placeholder="Ask something..."></textarea>
  <button onclick="sendChat()">Send</button>
  <div id="chat-output"></div>
</div>

<script>
/* -------------------------------
   SIDEBAR TOGGLE
-------------------------------- */
function toggleSidebar() {
  document.getElementById("sidebar").classList.toggle("open");
}

/* -------------------------------
   DARK MODE
-------------------------------- */
function toggleDarkMode() {
  document.body.classList.toggle("dark");
}

/* -------------------------------
   GENERATE STUDY PLAN
-------------------------------- */
function generatePlan() {
  const subjects = document.getElementById("subjects").value
    .split(",")
    .map(s => s.trim())
    .filter(s => s.length > 0);

  const hours = parseFloat(document.getElementById("hours").value);
  const difficulty = document.getElementById("difficulty").value;

  if (subjects.length === 0 || !hours) {
    document.getElementById("output").innerText = "⚠️ Please enter all fields.";
    return;
  }

  let multiplier = difficulty === "easy" ? 0.8 :
                   difficulty === "medium" ? 1 :
                   1.25;

  let timePerSubject = (hours / subjects.length) * multiplier;

  let plan = "📘 Your Personalized Study Plan:\n\n";
  subjects.forEach(sub => {
    plan += `• ${sub}: ${timePerSubject.toFixed(1)} hours/day\n`;
  });

  document.getElementById("output").innerText = plan;
}

/* -------------------------------
   PDF DOWNLOAD
-------------------------------- */
function downloadPDF() {
  const content = document.getElementById("output").innerText;
  if (!content || content === "Your plan will appear here...") {
    alert("Please generate a study plan first!");
    return;
  }

  const element = document.createElement('a');
  const file = new Blob([content], { type: 'text/plain' });
  element.href = URL.createObjectURL(file);
  element.download = "SmartStudyAI_StudyPlan.txt";
  element.click();
}

/* -------------------------------
   AI CHAT TOGGLE
-------------------------------- */
function toggleChat() {
  const chat = document.getElementById("chat-window");
  chat.style.display = (chat.style.display === "flex") ? "none" : "flex";
}

/* -------------------------------
   SIMPLE AI RESPONSE
-------------------------------- */
function sendChat() {
  const input = document.getElementById("chat-input").value;

  if (input.trim().length === 0) return;

  let response = "";

  if (input.includes("hello") || input.includes("hi")) {
    response = "Hello! How can I help you with your study plan?";
  } else if (input.includes("study")) {
    response = "Try studying in short intervals with breaks!";
  } else if (input.includes("time")) {
    response = "Plan your subjects based on difficulty and available time.";
  } else {
    response = "Thanks for your question! Try generating a study plan above.";
  }

  document.getElementById("chat-output").innerHTML +=
    `<p><b>You:</b> ${input}</p>
     <p><b>AI:</b> ${response}</p>`;

  document.getElementById("chat-input").value = "";
}

/* -------------------------------
   VOICE INPUT (Speech to Text)
-------------------------------- */
function startVoiceInput() {
  if (!('webkitSpeechRecognition' in window)) {
    alert("Speech recognition is not supported in this browser.");
    return;
  }

  const recognition = new webkitSpeechRecognition();
  recognition.lang = "en-US";
  recognition.interimResults = false;

  recognition.start();

  recognition.onresult = function(event) {
    const speechText = event.results[0][0].transcript;
    document.getElementById("subjects").value = speechText;
  };
}

/* -------------------------------
   LOADER REMOVAL
-------------------------------- */
window.onload = () => {
  setTimeout(() => {
    document.getElementById("loader").style.display = "none";
  }, 2000);
};
</script>
</body>
</html>
