<!DOCTYPE html>
<html>
<head>
  <title>Friends Chat</title>
  <script type="module">
    // Firebase setup
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.11.0/firebase-app.js";
    import { getDatabase, ref, push, onChildAdded } from "https://www.gstatic.com/firebasejs/10.11.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyBi4NoK1yQP7aRPKv4EPxGt1l2VthXyneg",
      authDomain: "friends-book-562a6.firebaseapp.com",
      databaseURL: "https://friends-book-562a6-default-rtdb.firebaseio.com",
      projectId: "friends-book-562a6",
      storageBucket: "friends-book-562a6.firebasestorage.app",
      messagingSenderId: "930560809468",
      appId: "1:930560809468:web:20396a03f4c4613fce7a79",
      measurementId: "G-0TFVL2ZG1C"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    let userName = "";

    window.onload = () => {
      userName = prompt("Enter your name:");
      if (!userName) userName = "Anonymous";
    };

    // Send message
    window.sendMessage = () => {
      const msgInput = document.getElementById("messageInput");
      const message = msgInput.value.trim();
      if (message !== "") {
        push(ref(db, "messages"), {
          name: userName,
          text: message
        });
        msgInput.value = "";
      }
    };

    // Display message
    const chatBox = document.getElementById("chatBox");
    onChildAdded(ref(db, "messages"), (data) => {
      const msg = data.val();
      const p = document.createElement("p");
      p.textContent = `${msg.name}: ${msg.text}`;
      chatBox.appendChild(p);
    });
  </script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    #chatBox { border: 1px solid #ccc; height: 300px; overflow-y: auto; padding: 10px; margin-bottom: 10px; }
    #messageInput { width: 70%; }
  </style>
</head>
<body>
  <h2>Friends Chat</h2>
  <div id="chatBox"></div>
  <input type="text" id="messageInput" placeholder="Type your message..." />
  <button onclick="sendMessage()">Send</button>
</body>
</html>
