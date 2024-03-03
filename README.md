#  Fullstack Chat Project
This project enables you to set up a full-stack chat for any frontend/backend combination, seamlessly connecting React, Vue, or Angular with your preferred backend.

Real-time Chat App with NodeJS, ReactJS, and ChatEngine.io
Build a feature-rich real-time chat application using Node JS, React JS, and ChatEngine.io. This application includes user authentication, socket connections, real-time messaging, image and file attachments, group chats, DMs, read receipts, and more!

## What We'll Build
FastAPI and ReactJS chat app with Chat Engine.

## Step 1: Setting up a NodeJS server
### Create a new project for the backend and frontend.
```bash
mkdir chat-app
cd chat-app
mkdir backend
cd backend
```

### Initiate a new NodeJS project.

```bash
npm init # Hit enter for every step
```

### Install dependencies.
```bash
npm i express cors axios
npm i --save-dev nodemon
```

### Update package.json with start script.
```json
"scripts": {
    "start": "nodemon index.js"
}
```

### Create index.js with basic server setup.

```javascript
// index.js
const express = require("express");
const cors = require("cors");

const app = express();
app.use(express.json());
app.use(cors({ origin: true }));

app.post("/authenticate", async (req, res) => {
  const { username } = req.body;
  return res.json({ username: username, secret: "sha256..." });
});

app.listen(3001);
```

### Start the server.
```bash
npm run start
```

## Step 2: Connecting Node JS to ChatEngine.io
Go to ChatEngine.io, sign up, and create a project. Note down the Project ID and Private Key.

Update the /authenticate function in index.js to connect to Chat Engine.

```javascript
// index.js
const axios = require("axios");

app.post("/authenticate", async (req, res) => {
  const { username } = req.body;
  try {
    const r = await axios.put(
      "https://api.chatengine.io/users/",
      { username: username, secret: username, first_name: username },
      { headers: { "Private-Key": "XXXX" } }
    );
    return res.status(r.status).json(r.data);
  } catch (e) {
    return res.status(e.response.status).json(e.response.data);
  }
});
```

## Step 3: Set up a React JS frontend
### Create a React JS project using Vite.
```bash

npm create vite@latest
```

Update the frontend/src/main.jsx and frontend/src/App.jsx files.

Create AuthPage.jsx and ChatsPage.jsx files.

Run the React JS app.

 ```bash
npm install
npm run dev
```

## Part 4: Connect React to Node JS and Chat Engine
### Install axios in the frontend.
```bash
npm install axios
```

### Update AuthPage.jsx to connect with the NodeJS server.
```javascript
// AuthPage.jsx
import axios from "axios";

const AuthPage = (props) => {
  const onSubmit = (e) => {
    e.preventDefault();
    const { value } = e.target[0];
    axios
      .post("http://localhost:3001/authenticate", { username: value })
      .then((r) => props.onAuth({ ...r.data, secret: value }))
      .catch((e) => console.log("Auth Error", e));
  };

  // Rest of the code
};

export default AuthPage;
```

### Install the react-chat-engine-pretty component.
```bash
npm install react-chat-engine-pretty
```

### Update ChatsPage.jsx to connect your React App to your Chat Engine project.
```javascript

// ChatsPage.jsx
import { PrettyChatWindow } from "react-chat-engine-pretty";

const ChatsPage = (props) => {
  return (
    <div className="background">
      <PrettyChatWindow
        projectId={import.meta.env.VITE_CHAT_ENGINE_PROJECT_ID}
        username={props.user.username}
        secret={props.user.secret}
      />
    </div>
  );
};

export default ChatsPage;
```
### Add VITE_CHAT_ENGINE_PROJECT_ID to frontend/.env.local and use your own Project ID.
```bash
VITE_CHAT_ENGINE_PROJECT_ID=XXX
```
