<!DOCTYPE html>
<html>
<head>
    <title>Telegram Login with Firestore</title>
    <!-- Telegram WebApp SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <!-- Firebase SDK (Firestore) -->
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-firestore-compat.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 20px;
            background-color: #f5f5f5;
        }
        #loginBtn {
            background: #0088cc;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 16px;
            border-radius: 5px;
            cursor: pointer;
        }
        #loginBtn:hover {
            background: #006699;
        }
    </style>
</head>
<body>
    <h1>🔐 Telegram Login</h1>
    <p>Only for Telegram users!</p>
    <button id="loginBtn">Login with Telegram</button>

    <script>
        // Firebase Configuration (Replace with YOUR details)
        const firebaseConfig = {
        apiKey: "AIzaSyCvGm2CvIZOR5AUeSLYOiFM-MVNKTOGWAU",
        authDomain: "crypto-hunters-d29cf.firebaseapp.com",
        projectId: "crypto-hunters-d29cf",
        storageBucket: "crypto-hunters-d29cf.firebasestorage.app",
        messagingSenderId: "470416764837",
        appId: "1:470416764837:web:f59b6b904a76d9d5ab024c"
        };
        
        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();

        // Telegram WebApp
        const tg = window.Telegram.WebApp;

        // Login Button Handler
        document.getElementById("loginBtn").addEventListener("click", async () => {
            const user = tg.initDataUnsafe.user;
            
            if (!user) {
                alert("Telegram user data not found!");
                return;
            }

            const userId = user.id;
            const username = user.username || `user_${userId}`;
            const firstName = user.first_name || "Anonymous";

            // Save to Firestore
            try {
                await db.collection("users").doc(userId.toString()).set({
                    username: username,
                    first_name: firstName,
                    last_login: new Date().toISOString()
                });
                alert("Login successful! Data saved to Firestore.");
                tg.close(); // Close Mini App
            } catch (error) {
                alert("Error saving to Firestore: " + error.message);
            }
        });

        // Expand Telegram WebApp to full screen
        tg.expand();
    </script>
</body>
</html>
