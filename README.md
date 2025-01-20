
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>✭ Schnansch ✭</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Bubblegum+Sans&display=swap');

        body {
            background-color: pink;
            font-family: 'Courier New', Courier, monospace;
            color: black;
            margin: 0;
            padding: 0;
            overflow-y: auto;
        }
        h1 {
            font-family: 'Bubblegum Sans', cursive;
            font-size: 3rem;
            text-align: center;
            color: white;
            margin: 20px 0;
        }
        #chat-container {
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            height: calc(100vh - 100px);
            width: 100%;
            padding: 20px;
            box-sizing: border-box;
        }
        #messages {
            flex-grow: 1;
            overflow-y: auto;
            background: rgba(255, 255, 255, 0.7);
            border-radius: 10px;
            padding: 10px;
            margin-bottom: 20px;
            font-family: 'Courier New', Courier, monospace;
            text-align: left;
        }
        #messages div {
            margin: 5px 0;
        }
        #input-area {
            display: flex;
            gap: 10px;
        }
        #input-area input {
            flex-grow: 1;
            padding: 10px;
            font-size: 1rem;
            border-radius: 5px;
            border: none;
            font-family: 'Courier New', Courier, monospace;
        }
        #input-area button {
            padding: 10px;
            font-size: 1rem;
            border-radius: 5px;
            border: none;
            cursor: pointer;
            background-color: white;
            color: black;
            font-family: 'Courier New', Courier, monospace;
        }
        #gif-picker {
            padding: 10px;
            font-size: 1rem;
            border-radius: 5px;
            border: none;
            cursor: pointer;
            background-color: white;
            color: black;
            font-family: 'Courier New', Courier, monospace;
        }
    </style>
</head>
<body>
    <h1>Pink Chatroom</h1>
    <div id="chat-container">
        <div id="messages"></div>
        <div id="input-area">
            <input type="text" id="message-input" placeholder="Schreib etwas..." autofocus>
            <button onclick="sendMessage()">Senden</button>
            <button id="gif-picker" onclick="openGifPicker()">GIF</button>
        </div>
    </div>

    <script>
        const messagesDiv = document.getElementById('messages');
        const messageInput = document.getElementById('message-input');

        let username = localStorage.getItem('chatUsername');
        if (!username) {
            username = prompt('Wie heißt du?') || 'Anonym';
            localStorage.setItem('chatUsername', username);
        }

        function sendMessage() {
            const message = messageInput.value.trim();
            if (message) {
                addMessage(username, message);
                messageInput.value = '';
                messageInput.focus();

                // Save messages to local storage when the user sends a new message
                const currentMessages = JSON.parse(localStorage.getItem('chatMessages')) || [];
                currentMessages.push({ user: username, text: message });
                localStorage.setItem('chatMessages', JSON.stringify(currentMessages));
            }
        }

        function addMessage(user, text) {
            const messageElement = document.createElement('div');
            messageElement.innerHTML = `<strong>${user}:</strong> ${text}`;
            messagesDiv.appendChild(messageElement);
            messagesDiv.scrollTop = messagesDiv.scrollHeight;
        }

        function openGifPicker() {
            const gifUrl = prompt('Füge eine URL für dein GIF ein:');
            if (gifUrl) {
                addMessage(username, `<img src="${gifUrl}" alt="GIF" style="max-width: 100%; height: auto;">`);
            }
        }

        // Load messages from local storage only (but do not save them again)
        const savedMessages = JSON.parse(localStorage.getItem('chatMessages')) || [];
        savedMessages.forEach(msg => addMessage(msg.user, msg.text));

        // Focus on input field on load
        messageInput.focus();

        // Add event listener for Enter key to send message
        messageInput.addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
                sendMessage();
            }
        });
    </script>
</body>
</html>
