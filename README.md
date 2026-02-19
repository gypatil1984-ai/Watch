
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Digital Watch</title>

<style>
    body {
        margin: 0;
        font-family: 'Segoe UI', sans-serif;
        background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        color: white;
    }

    .watch {
        background: #111;
        padding: 40px;
        border-radius: 20px;
        text-align: center;
        box-shadow: 0 0 30px rgba(0,255,255,0.6);
        width: 320px;
    }

    .time {
        font-size: 48px;
        font-weight: bold;
        margin-bottom: 10px;
    }

    .date {
        font-size: 18px;
        margin-bottom: 20px;
        opacity: 0.8;
    }

    .ai-message {
        font-size: 16px;
        margin-top: 15px;
        color: #00ffff;
    }

    button {
        margin-top: 20px;
        padding: 10px 15px;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        background: #00ffff;
        color: black;
        font-weight: bold;
    }

    button:hover {
        background: #00cccc;
    }
</style>
</head>
<body>

<div class="watch">
    <div class="time" id="time">00:00:00</div>
    <div class="date" id="date">Loading date...</div>
    <div class="ai-message" id="aiMessage">Hello!</div>
    <button onclick="startVoiceAI()">🎤 Talk to AI</button>
</div>

<script>
function updateTime() {
    const now = new Date();

    let hours = now.getHours();
    let minutes = now.getMinutes();
    let seconds = now.getSeconds();

    hours = hours < 10 ? "0" + hours : hours;
    minutes = minutes < 10 ? "0" + minutes : minutes;
    seconds = seconds < 10 ? "0" + seconds : seconds;

    document.getElementById("time").textContent = 
        hours + ":" + minutes + ":" + seconds;

    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    document.getElementById("date").textContent = 
        now.toLocaleDateString(undefined, options);

    updateAIGreeting(hours);
}

function updateAIGreeting(hour) {
    let message = "";

    if (hour < 12) {
        message = "🤖 Good Morning! Stay productive!";
    } else if (hour < 18) {
        message = "🤖 Good Afternoon! Keep going!";
    } else {
        message = "🤖 Good Evening! Time to relax!";
    }

    document.getElementById("aiMessage").textContent = message;
}

setInterval(updateTime, 1000);
updateTime();

// Simple AI Voice Assistant
function startVoiceAI() {
    const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.lang = "en-US";

    recognition.start();

    recognition.onresult = function(event) {
        const speech = event.results[0][0].transcript.toLowerCase();
        let response = "";

        if (speech.includes("time")) {
            response = "The current time is " + document.getElementById("time").textContent;
        } else if (speech.includes("date")) {
            response = "Today's date is " + document.getElementById("date").textContent;
        } else {
            response = "Sorry, I did not understand.";
        }

        speak(response);
    };
}

function speak(text) {
    const speech = new SpeechSynthesisUtterance(text);
    speech.lang = "en-US";
    window.speechSynthesis.speak(speech);
}
</script>

</body>
</html>
