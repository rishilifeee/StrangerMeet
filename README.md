<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StrangerMeet • Random Video Chat</title>
    <base href="/StrangerMeet/">
    <style>
        :root {
            --primary: #6C63FF;
            --secondary: #FF6584;
            --dark: #1E1E2C;
            --light: #F5F7FA;
            --success: #4CAF50;
        }
        
        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            margin: 0;
            padding: 0;
            background-color: var(--light);
            color: var(--dark);
            transition: all 0.3s ease;
        }
        
        body.dark-mode {
            background-color: var(--dark);
            color: var(--light);
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
            position: relative;
        }
        
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 2rem;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--primary);
        }
        
        .logo-icon {
            width: 2rem;
            height: 2rem;
            background-color: var(--primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
        }
        
        .theme-toggle {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: var(--dark);
        }
        
        .dark-mode .theme-toggle {
            color: var(--light);
        }
        
        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 2rem;
        }
        
        .video-container {
            position: relative;
            border-radius: 1rem;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            background-color: var(--dark);
            aspect-ratio: 16/9;
        }
        
        .dark-mode .video-container {
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }
        
        video {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        #local-video {
            transform: scaleX(-1);
        }
        
        .video-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            z-index: 10;
        }
        
        .connection-status {
            margin-top: 1rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .connection-dot {
            width: 0.8rem;
            height: 0.8rem;
            border-radius: 50%;
            background-color: #FF5252;
        }
        
        .connection-dot.connected {
            background-color: var(--success);
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
        
        .chat-container {
            display: flex;
            flex-direction: column;
            height: 100%;
        }
        
        .chat-box {
            flex-grow: 1;
            border-radius: 1rem;
            padding: 1.5rem;
            background-color: white;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.05);
            overflow-y: auto;
            margin-bottom: 1rem;
        }
        
        .dark-mode .chat-box {
            background-color: #2A2A3C;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
        }
        
        .message {
            margin-bottom: 1rem;
            padding: 0.8rem 1rem;
            border-radius: 1rem;
            max-width: 80%;
            word-wrap: break-word;
        }
        
        .message.you {
            background-color: var(--primary);
            color: white;
            margin-left: auto;
            border-bottom-right-radius: 0.3rem;
        }
        
        .message.stranger {
            background-color: #EFF0F6;
            margin-right: auto;
            border-bottom-left-radius: 0.3rem;
        }
        
        .dark-mode .message.stranger {
            background-color: #3A3A4D;
            color: white;
        }
        
        .message-input-container {
            display: flex;
            gap: 0.5rem;
        }
        
        #message-input {
            flex-grow: 1;
            padding: 1rem;
            border-radius: 0.5rem;
            border: 1px solid #ddd;
            font-size: 1rem;
        }
        
        .dark-mode #message-input {
            background-color: #2A2A3C;
            border-color: #3A3A4D;
            color: white;
        }
        
        .send-btn {
            padding: 1rem 1.5rem;
            background-color: var(--primary);
            color: white;
            border: none;
            border-radius: 0.5rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .send-btn:hover {
            background-color: #5A52E0;
            transform: translateY(-2px);
        }
        
        .action-buttons {
            display: flex;
            gap: 0.5rem;
            margin-top: 1rem;
        }
        
        .action-btn {
            flex-grow: 1;
            padding: 0.8rem;
            border-radius: 0.5rem;
            border: none;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .connect-btn {
            background-color: var(--primary);
            color: white;
        }
        
        .disconnect-btn {
            background-color: var(--secondary);
            color: white;
        }
        
        .interests-btn {
            background-color: #EFF0F6;
        }
        
        .dark-mode .interests-btn {
            background-color: #3A3A4D;
            color: white;
        }

        /* Ad Container Styles */
        .ad-container {
            background-color: #f5f5f5;
            border: 1px solid #ddd;
            border-radius: 8px;
            overflow: hidden;
            margin: 1rem 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 90px;
        }

        .dark-mode .ad-container {
            background-color: #2a2a3c;
            border-color: #3a3a4d;
        }

        .ad-placeholder {
            padding: 1rem;
            text-align: center;
            color: #999;
        }

        .side-ad {
            width: 160px;
            height: 600px;
            position: fixed;
            right: 2rem;
            top: 50%;
            transform: translateY(-50%);
        }

        .bottom-ad {
            width: 728px;
            height: 90px;
            margin: 0 auto;
        }

        @media (max-width: 1200px) {
            .side-ad {
                display: none;
            }
        }

        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .bottom-ad {
                width: 100%;
                height: auto;
                min-height: 50px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <div class="logo-icon">S</div>
                <span>StrangerMeet</span>
            </div>
            <button class="theme-toggle" id="theme-toggle">🌓</button>
        </header>
        
        <!-- Ad Container (Sidebar) -->
        <div class="ad-container side-ad" id="side-ad">
            <div class="ad-placeholder">
                <span>Advertisement</span>
            </div>
        </div>
        
        <main class="main-content">
            <div class="video-container">
                <video id="local-video" autoplay muted></video>
                <video id="remote-video" autoplay></video>
                
                <div class="video-overlay" id="video-overlay">
                    <h2>Ready to Connect</h2>
                    <p>Click "Find Stranger" to start a random chat</p>
                    <div class="connection-status">
                        <div class="connection-dot"></div>
                        <span>Disconnected</span>
                    </div>
                </div>
            </div>
            
            <div class="chat-container">
                <div class="chat-box" id="chat-box">
                    <div class="message stranger">
                        Welcome to StrangerMeet! Your conversations are private and anonymous.
                    </div>
                    <div class="message stranger">
                        Select your interests to find better matches.
                    </div>
                </div>
                
                <div class="message-input-container">
                    <input type="text" id="message-input" placeholder="Type your message...">
                    <button class="send-btn" id="send-btn">Send</button>
                </div>
                
                <div class="action-buttons">
                    <button class="action-btn connect-btn" id="connect-btn">Find Stranger</button>
                    <button class="action-btn disconnect-btn" id="disconnect-btn" disabled>Disconnect</button>
                </div>
            </div>
        </main>
        
        <!-- Ad Container (Bottom Banner) -->
        <div class="ad-container bottom-ad" id="bottom-ad">
            <div class="ad-placeholder">
                <span>Advertisement</span>
            </div>
        </div>
    </div>

    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-database-compat.js"></script>
    
    <script>
        // Firebase Configuration (Replace with your own)
        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_AUTH_DOMAIN",
            databaseURL: "YOUR_DATABASE_URL",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_STORAGE_BUCKET",
            messagingSenderId: "YOUR_SENDER_ID",
            appId: "YOUR_APP_ID"
        };

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();

        // DOM Elements
        const themeToggle = document.getElementById('theme-toggle');
        const localVideo = document.getElementById('local-video');
        const remoteVideo = document.getElementById('remote-video');
        const videoOverlay = document.getElementById('video-overlay');
        const connectionDot = document.querySelector('.connection-dot');
        const connectionText = document.querySelector('.connection-status span');
        const chatBox = document.getElementById('chat-box');
        const messageInput = document.getElementById('message-input');
        const sendBtn = document.getElementById('send-btn');
        const connectBtn = document.getElementById('connect-btn');
        const disconnectBtn = document.getElementById('disconnect-btn');
        const sideAd = document.getElementById('side-ad');
        const bottomAd = document.getElementById('bottom-ad');

        // App State
        let localStream;
        let remoteStream;
        let currentChatId;
        let isConnected = false;

        // Initialize the app
        function init() {
            setupThemeToggle();
            setupEventListeners();
            initWebcam();
            loadAds();
        }

        // Set up theme toggle
        function setupThemeToggle() {
            const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
            if (prefersDark) document.body.classList.add('dark-mode');
            
            themeToggle.addEventListener('click', () => {
                document.body.classList.toggle('dark-mode');
            });
        }

        // Set up event listeners
        function setupEventListeners() {
            sendBtn.addEventListener('click', sendMessage);
            messageInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') sendMessage();
            });
            
            connectBtn.addEventListener('click', findStranger);
            disconnectBtn.addEventListener('click', disconnect);
        }

        // Initialize webcam
        async function initWebcam() {
            try {
                localStream = await navigator.mediaDevices.getUserMedia({ 
                    video: true, 
                    audio: true 
                });
                localVideo.srcObject = localStream;
            } catch (err) {
                console.error("Error accessing media devices:", err);
                addSystemMessage("Could not access your camera/microphone. Please check permissions.");
            }
        }

        // Find a random stranger
        function findStranger() {
            if (isConnected) return;
            
            currentChatId = `chat_${Date.now()}`;
            
            // Set up chat room
            database.ref('chats/' + currentChatId).set({
                user1: "connected",
                user2: "waiting",
                createdAt: firebase.database.ServerValue.TIMESTAMP
            });
            
            // Listen for messages
            database.ref('chats/' + currentChatId + '/messages').on('child_added', (snapshot) => {
                const message = snapshot.val();
                displayMessage(message.text, message.sender);
            });
            
            // Update UI
            updateConnectionStatus(true);
            addSystemMessage("Looking for a stranger...");
            
            // Check if someone joins within 30 seconds
            setTimeout(() => {
                if (!isConnected) {
                    database.ref('chats/' + currentChatId).once('value').then(snapshot => {
                        if (snapshot.val().user2 === "waiting") {
                            addSystemMessage("Couldn't find a match. Try again!");
                            database.ref('chats/' + currentChatId).remove();
                            updateConnectionStatus(false);
                        }
                    });
                }
            }, 30000);
        }

        // Disconnect from current chat
        function disconnect() {
            if (!isConnected) return;
            
            database.ref('chats/' + currentChatId).remove();
            updateConnectionStatus(false);
            addSystemMessage("You disconnected from the chat");
            
            // Reset video elements
            if (remoteVideo.srcObject) {
                remoteVideo.srcObject.getTracks().forEach(track => track.stop());
                remoteVideo.srcObject = null;
            }
        }

        // Send a message
        function sendMessage() {
            if (!isConnected || !messageInput.value.trim()) return;
            
            const message = {
                text: messageInput.value.trim(),
                sender: "You",
                timestamp: firebase.database.ServerValue.TIMESTAMP
            };
            
            database.ref('chats/' + currentChatId + '/messages').push(message);
            displayMessage(message.text, message.sender);
            messageInput.value = "";
        }

        // Display a message in chat
        function displayMessage(text, sender) {
            const messageElement = document.createElement('div');
            messageElement.className = `message ${sender === "You" ? "you" : "stranger"}`;
            messageElement.textContent = text;
            chatBox.appendChild(messageElement);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        // Add system message
        function addSystemMessage(text) {
            const messageElement = document.createElement('div');
            messageElement.className = 'message stranger';
            messageElement.innerHTML = `<em>${text}</em>`;
            chatBox.appendChild(messageElement);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        // Update connection status UI
        function updateConnectionStatus(connected) {
            isConnected = connected;
            
            if (connected) {
                connectionDot.classList.add('connected');
                connectionText.textContent = "Connected";
                videoOverlay.style.display = "none";
                connectBtn.disabled = true;
                disconnectBtn.disabled = false;
                toggleAds(false); // Hide ads during chat
            } else {
                connectionDot.classList.remove('connected');
                connectionText.textContent = "Disconnected";
                videoOverlay.style.display = "flex";
                connectBtn.disabled = false;
                disconnectBtn.disabled = true;
                currentChatId = null;
                toggleAds(true); // Show ads when disconnected
            }
        }

        // Ad Management System
        function loadAds() {
            // Sample ad rotation
            const ads = {
                side: [
                    '<img src="https://via.placeholder.com/160x600/FF0000/FFFFFF?text=Ad+1" alt="Ad">',
                    '<img src="https://via.placeholder.com/160x600/00FF00/FFFFFF?text=Ad+2" alt="Ad">'
                ],
                bottom: [
                    '<img src="https://via.placeholder.com/728x90/0000FF/FFFFFF?text=Banner+Ad+1" alt="Ad">',
                    '<img src="https://via.placeholder.com/728x90/FFFF00/000000?text=Banner+Ad+2" alt="Ad">'
                ]
            };

            function rotateAds() {
                if (sideAd) {
                    const randomAd = ads.side[Math.floor(Math.random() * ads.side.length)];
                    sideAd.innerHTML = randomAd;
                }
                
                if (bottomAd) {
                    const randomAd = ads.bottom[Math.floor(Math.random() * ads.bottom.length)];
                    bottomAd.innerHTML = randomAd;
                }
            }

            // Toggle ad visibility
            function toggleAds(show) {
                [sideAd, bottomAd].forEach(ad => {
                    if (ad) ad.style.display = show ? 'flex' : 'none';
                });
            }

            // Initial ad load
            rotateAds();
            
            // Rotate ads every 30 seconds
            setInterval(rotateAds, 30000);
        }

        // Start the application
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
