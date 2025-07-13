<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>פיניקס – העוזר החכם שלך</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
    }
    #phoenix-chat-btns {
      position: fixed;
      bottom: 20px;
      right: 20px;
      display: flex;
      flex-direction: column;
      gap: 10px;
      z-index: 9999;
    }
    .phoenix-btn {
      background: #FF8C00;
      color: white;
      border: none;
      padding: 10px 16px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: bold;
    }
    #phoenix-chat {
      position: fixed;
      bottom: 80px;
      right: 20px;
      width: 320px;
      max-height: 400px;
      background: #1e1e1e;
      color: #f1f1f1;
      border-radius: 16px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.5);
      font-family: Arial;
      display: none;
      flex-direction: column;
      z-index: 9999;
    }
    #phoenix-chat-header {
      background: #ff6a00;
      color: white;
      padding: 12px;
      border-radius: 16px 16px 0 0;
      font-weight: bold;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    #phoenix-chat-header img {
      width: 24px;
      height: 24px;
      border-radius: 50%;
    }
    #phoenix-chat-messages {
      flex: 1;
      overflow-y: auto;
      padding: 10px;
    }
    #phoenix-chat-input {
      display: flex;
      border-top: 1px solid #444;
    }
    #phoenix-chat-input input {
      flex: 1;
      background: #2e2e2e;
      color: #fff;
      border: none;
      padding: 10px;
      border-bottom-left-radius: 16px;
    }
    #phoenix-chat-input button {
      background: #ff6a00;
      color: white;
      border: none;
      padding: 10px 16px;
      border-bottom-right-radius: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div id="phoenix-chat-btns">
    <button class="phoenix-btn" onclick="openPhoenix('generic')">🧠 עוזר חכם</button>
    <button class="phoenix-btn" onclick="openPhoenix('code')">👨‍💻 קידוד ופיתוח</button>
    <button class="phoenix-btn" onclick="openPhoenix('support')">❤️ תמיכה אישית</button>
    <button class="phoenix-btn" onclick="openPhoenix('community')">👥 קהילה</button>
    <button class="phoenix-btn" onclick="openPhoenix('trainer')">🏋️ תוכנית אימונים</button>
  </div>

  <div id="phoenix-chat">
    <div id="phoenix-chat-header">
      <img src="https://i.imgur.com/NsMaCQU.png" alt="פיניקס"/>
      <span>🦅 פיניקס</span>
    </div>
    <div id="phoenix-chat-messages"></div>
    <div id="phoenix-chat-input">
      <input type="text" placeholder="כתוב שאלה..." id="phoenix-input"/>
      <button onclick="sendPhoenixMessage()">שלח</button>
    </div>
  </div>

  <script>
    async function askPhoenix(message) {
      const res = await fetch("https://phoenix-chat.live.workers.dev", {
        method: "POST",
        headers: {"Content-Type": "application/json"},
        body: JSON.stringify({ message })
      });
      const data = await res.json();
      return data.reply;
    }

    let currentMode = null;

    async function openPhoenix(mode) {
      currentMode = mode;
      const chat = document.getElementById("phoenix-chat");
      const msgs = document.getElementById("phoenix-chat-messages");
      msgs.innerHTML = '';
      chat.style.display = 'flex';
      const intros = {
        generic: 'היי! אני פיניקס, העוזר החכם שלך.\nבמה אוכל לעזור לך היום?',
        code: 'מעולה! ספר לי מה אתה מנסה לפתח –\nאני כאן לעזור לך עם קוד, רעיונות או כל שאלה שיש לך 💡',
        support: 'אני כאן בשבילך.\nרוצה לספר לי מה עובר עליך או במה אתה צריך תמיכה?',
        community: 'ברוך הבא לקהילה! אני שמח שבחרת להצטרף 🙌\nתרצה להתחבר לאנשים דומים?',
        trainer: 'היי, אני פיניקס – המאמן האישי שלך 🔥\nבוא נבנה יחד תוכנית אימונים שתתאים בדיוק לך. אני אשאל אותך כמה שאלות קצרות 💪'
      };
      msgs.innerHTML += `<div><b>פיניקס:</b> ${intros[mode]}</div>`;
    }

    async function sendPhoenixMessage() {
      const input = document.getElementById("phoenix-input");
      const msg = input.value;
      if (!msg) return;
      const msgs = document.getElementById("phoenix-chat-messages");
      msgs.innerHTML += `<div><b>אתה:</b> ${msg}</div>`;
      input.value = '';
      const reply = await askPhoenix(msg);
      msgs.innerHTML += `<div><b>פיניקס:</b> ${reply}</div>`;
      msgs.scrollTop = msgs.scrollHeight;
    }
  </script>
</body>
</html>
