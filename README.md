# ERROR-
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jarvis - Personal AI Assistant</title>
  <style>
    body {
      background: #0f172a;
      color: #e2e8f0;
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      padding: 20px;
    }
    h1 { color: #38bdf8; }
    #chat-box {
      width: 90%;
      max-width: 800px;
      height: 60vh;
      background: #1e293b;
      border-radius: 10px;
      padding: 15px;
      overflow-y: auto;
      margin-bottom: 10px;
      box-shadow: 0 0 15px rgba(56,189,248,0.3);
    }
    .msg {
      margin: 10px 0;
      line-height: 1.5;
    }
    .user { color: #a5f3fc; }
    .bot { color: #f9fafb; }
    #input-box {
      display: flex;
      width: 90%;
      max-width: 800px;
    }
    input {
      flex: 1;
      padding: 10px;
      border: none;
      border-radius: 8px 0 0 8px;
      outline: none;
      font-size: 16px;
    }
    button {
      padding: 10px 20px;
      border: none;
      border-radius: 0 8px 8px 0;
      background: #38bdf8;
      color: #0f172a;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover {
      background: #0ea5e9;
    }
  </style>
</head>
<body>
  <h1>🤖 Jarvis - Multilingual Personal Assistant</h1>
  <div id="chat-box"></div>
  <div id="input-box">
    <input id="user-input" type="text" placeholder="Ask me anything..." />
    <button onclick="sendMessage()">Send</button>
  </div>

  <script>
    const OPENAI_API_KEY = "YOUR_API_KEY_HERE"; // ← Replace this with your OpenAI API key
    const MODEL = "gpt-4o-mini"; // or gpt-5 if available

    const chatBox = document.getElementById('chat-box');
    const userInput = document.getElementById('user-input');

    async function sendMessage() {
      const message = userInput.value.trim();
      if (!message) return;
      appendMessage('user', message);
      userInput.value = '';

      appendMessage('bot', 'Thinking...');

      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${OPENAI_API_KEY}`
        },
        body: JSON.stringify({
          model: MODEL,
          messages: [
            { role: "system", content: "You are Jarvis, an intelligent personal assistant who knows 50+ languages and all programming languages. Be helpful, concise, and friendly." },
            { role: "user", content: message }
          ]
        })
      });

      const data = await response.json();
      chatBox.lastChild.remove(); // remove "Thinking..."
      appendMessage('bot', data.choices[0].message.content);
    }

    function appendMessage(sender, text) {
      const msg = document.createElement('div');
      msg.className = `msg ${sender}`;
      msg.innerText = (sender === 'user' ? '🧑: ' : '🤖: ') + text;
      chatBox.appendChild(msg);
      chatBox.scrollTop = chatBox.scrollHeight;
    }

    userInput.addEventListener("keypress", (e) => {
      if (e.key === "Enter") sendMessage();
    });
  </script>
</body>
</html>
