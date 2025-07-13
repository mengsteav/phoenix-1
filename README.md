# Save the latest, verified, updated version of the Phoenix assistant code
final_html_code = """
<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>פיניקס – העוזר החכם שלך</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #0d1117;
      color: #fff;
    }
    #chat-container {
      position: fixed;
      bottom: 20px;
      right: 20px;
      width: 350px;
      height: 500px;
      background: #161b22;
      border-radius: 16px;
      box-shadow: 0 0 20px rgba(255,106,0,0.4);
      display: flex;
      flex-direction: column;
      overflow: hidden;
      z-index: 9999;
    }
    #chat-header {
      background: #ff6a00;
      padding: 14px;
      font-size: 18px;
      font-weight: bold;
      text-align: center;
    }
    #chat-messages {
      flex: 1;
      padding: 12px;
      overflow-y: auto;
      font-size: 15px;
      line-height: 1.6;
    }
    #chat-input {
      display: flex;
      border-top: 1px solid #30363d;
    }
    #chat-input input {
      flex: 1;
      padding: 12px;
      border: none;
      outline: none;
      background: #21262d;
      color: #fff;
      font-size: 15px;
    }
    #chat-input button {
      padding: 12px 18px;
      border: none;
      background: #ff6a00;
      color: white;
      cursor: pointer;
      font-weight: bold;
    }
  </style>
</head>
<body>

<div id="chat-container">
  <div id="chat-header">🦅 פיניקס – העוזר החכם שלך</div>
  <div id="chat-messages"></div>
  <div id="chat-input">
    <input type="text" id="user-input" placeholder="כתוב כאן..." />
    <button onclick="sendMessage()">שלח</button>
  </div>
</div>

<script>
  const messages = document.getElementById("chat-messages");
  const input = document.getElementById("user-input");
  let username = null;

  const appendMessage = (sender, text) => {
    const msg = document.createElement("div");
    msg.innerHTML = `<b>${sender}:</b> ${text}`;
    messages.appendChild(msg);
    messages.scrollTop = messages.scrollHeight;
  };

  const askPhoenix = async (message) => {
    try {
      const res = await fetch("https://phoenix-chat.live.workers.dev", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ message })
      });
      const data = await res.json();
      return data.reply;
    } catch (e) {
      return "מצטער, הייתה בעיה זמנית. נסה שוב.";
    }
  };

  const sendMessage = async () => {
    const text = input.value.trim();
    if (!text) return;
    appendMessage("אתה", text);
    input.value = "";

    if (!username && text.includes("אני")) {
      const parts = text.split("אני");
      if (parts.length > 1) {
        username = parts[1].trim().split(" ")[0];
        appendMessage("פיניקס", `שלום ${username}! איך אפשר לעזור?`);
        return;
      }
    }

    const reply = await askPhoenix(username ? `${username}: ${text}` : text);
    appendMessage("פיניקס", reply);
  };

  appendMessage("פיניקס", "שלום וברוך הבא! איך קוראים לך?");
</script>

</body>
</html>
"""

final_file_path = "/mnt/data/phoenix_final_index.html"
with open(final_file_path, "w", encoding="utf-8") as f:
    f.write(final_html_code)

final_file_path
