# How to Build a Study Timer Web App (Using JavaScript + Local Storage)

Staying focused is hard.

We sit down to study or work, but distractions are everywhere—phones, notifications, even our thoughts. That’s why a simple **study timer** can make a huge difference. It helps you stay accountable, track how long you’ve studied, and take breaks at the right time.

What if you could **build your own study timer app**? One that works offline, looks clean, and even remembers your progress?

In this guide, we’ll create a **study timer web app** using **HTML, CSS, JavaScript**, and **localStorage**—with no libraries or frameworks. It’s simple, but powerful.

Let’s get started.

---

## **What Will This Timer App Do?**

* Start/pause a 25-minute study session
* Store completed sessions in the browser
* Show how much time you've studied today
* Work even if the user refreshes the page

---

## **Step 1: Set Up Basic HTML**

Create an `index.html` file and add this:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Study Timer</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="container">
    <h1>Study Timer</h1>
    <div id="timer">25:00</div>
    <button id="startBtn">Start</button>
    <button id="resetBtn">Reset</button>
    <p>Today's Study Time: <span id="studyTime">0</span> minutes</p>
  </div>
  <script src="script.js"></script>
</body>
</html>
```

This sets up a simple structure: a title, timer display, two buttons, and a tracker.

---

## **Step 2: Add Some Styling (CSS)**

Create a `style.css` file:

```css
body {
  background: #f4f4f4;
  font-family: sans-serif;
  text-align: center;
  padding: 40px;
}

.container {
  max-width: 400px;
  margin: auto;
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

#timer {
  font-size: 48px;
  margin: 20px 0;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  margin: 5px;
  cursor: pointer;
}

p {
  font-size: 18px;
  margin-top: 20px;
}
```

It’s minimal, but visually clear and mobile-friendly.

---

## **Step 3: JavaScript Logic (Timer + Storage)**

Create a `script.js` file and paste this code:

```js
const timerDisplay = document.getElementById('timer');
const startBtn = document.getElementById('startBtn');
const resetBtn = document.getElementById('resetBtn');
const studyTimeEl = document.getElementById('studyTime');

let timer;
let isRunning = false;
let timeLeft = 25 * 60; // 25 minutes

// Load previous study time
let todayKey = new Date().toISOString().split('T')[0];
let totalTime = parseInt(localStorage.getItem(todayKey)) || 0;
studyTimeEl.textContent = totalTime;

// Format time in mm:ss
function formatTime(seconds) {
  const m = Math.floor(seconds / 60).toString().padStart(2, '0');
  const s = (seconds % 60).toString().padStart(2, '0');
  return `${m}:${s}`;
}

// Update display
function updateDisplay() {
  timerDisplay.textContent = formatTime(timeLeft);
}

// Countdown function
function startCountdown() {
  if (isRunning) return;
  isRunning = true;

  timer = setInterval(() => {
    if (timeLeft <= 0) {
      clearInterval(timer);
      isRunning = false;
      addStudySession(25); // full session
      alert("Session complete!");
      timeLeft = 25 * 60;
      updateDisplay();
      return;
    }

    timeLeft--;
    updateDisplay();
  }, 1000);
}

// Add study session to localStorage
function addStudySession(minutes) {
  let current = parseInt(localStorage.getItem(todayKey)) || 0;
  current += minutes;
  localStorage.setItem(todayKey, current);
  studyTimeEl.textContent = current;
}

// Reset timer
function resetTimer() {
  clearInterval(timer);
  isRunning = false;
  timeLeft = 25 * 60;
  updateDisplay();
}

startBtn.addEventListener('click', startCountdown);
resetBtn.addEventListener('click', resetTimer);

// Initial render
updateDisplay();
```

---

## **How It Works**

* When you hit **Start**, it begins a countdown from 25 minutes.
* If it completes, it shows an alert and adds **25 minutes** to your local total.
* Your daily study time is stored using `localStorage` and updates automatically.
* It resets cleanly when you hit the **Reset** button.

---

## **Bonus: Extend the App Later**

Once you’re done, you can add:

* **Custom session lengths** (focus, short break, long break)
* **Sound alerts**
* **Dark mode**
* **Daily/weekly charts using Chart.js**
* **Backend sync for cross-device tracking**

This simple version is just the foundation. You can grow it into your own personalized productivity tool.

---

## **Final Thoughts – Small Tools, Big Impact**

Sometimes, small tools create big change. A study timer might seem like a tiny app—but it helps build discipline, improves focus, and makes study sessions intentional.

By building it yourself, you’ve not only made a tool—but you've taken charge of your own learning and productivity.

Whether you're preparing for an exam, writing code, or studying something new…
This timer is yours. It works the way you want it to.

And that’s what makes it powerful.
