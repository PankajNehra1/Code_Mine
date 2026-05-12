What is Code Mine?
Code Mine is a real-time collaborative code editor where multiple users can join a shared room and write, edit, and review code simultaneously — all changes are synced live across every connected client. Built for pair programming, technical interviews, and team collaboration.

Features

Live Collaboration — Multiple users edit the same file in real time; every keystroke syncs instantly across all connected clients
Room-Based Sessions — Create or join a named room to start a collaborative session
Syntax Highlighting — Full code editor experience with language-aware highlighting
Persistent Sessions — Session state stored in MongoDB so rooms survive server restarts
Dockerised Setup — One command to spin up the full stack (client, server, database) with Docker Compose
TypeScript End-to-End — Strongly typed across both client and server for reliability and maintainability


Tech Stack
LayerTechnologyFrontendTypeScript, Vite, ReactBackendNode.js, Express, Socket.IODatabaseMongoDBContainerisationDocker, Docker ComposeReal-Time SyncWebSockets (Socket.IO)

Architecture
Browser Client (Vite + TypeScript)
        │
        └── WebSocket (Socket.IO) ──► Node.js Server (port 3000)
                                              │
                                        MongoDB (port 27017)
                                     (room & session persistence)
Key design decisions:

All editor changes are broadcast as Socket.IO events — the server acts as a relay, pushing diffs to every client in the same room instantly
MongoDB stores room state so that a user joining mid-session sees the current document, not a blank editor
The client is served as a static build via Nginx inside Docker, keeping the production setup clean and portable


Getting Started
Prerequisites

Docker and Docker Compose installed
OR: Node.js v18+ and MongoDB (for running locally without Docker)


Option 1 — Run with Docker (Recommended)
bash# Clone the repository
git clone https://github.com/PankajNehra1/Code_Mine.git
cd Code_Mine

# Create environment file for the server
cp server/.env.example server/.env
# Edit server/.env with your config (MongoDB URI, port, etc.)

# Start all services
docker-compose up --build
ServiceURLFrontendhttp://localhost:5173Backend APIhttp://localhost:3000MongoDBlocalhost:27017

Option 2 — Run Locally (Without Docker)
bash# Clone the repository
git clone https://github.com/PankajNehra1/Code_Mine.git
cd Code_Mine

# Install and start the server
cd server
npm install
npm run dev

# In a new terminal, install and start the client
cd client
npm install
npm run dev
Make sure MongoDB is running locally on port 27017.

Environment Variables
Create a .env file in the /server directory:
envPORT=3000
MONGO_URI=mongodb://mongo:27017/codesync

Project Structure
Code_Mine/
├── client/                  # TypeScript + Vite frontend
│   ├── src/
│   │   ├── components/      # Editor, Sidebar, RoomJoin UI
│   │   ├── hooks/           # Socket event hooks
│   │   └── pages/           # Route-level pages
│   └── Dockerfile
├── server/                  # Node.js + Express backend
│   ├── src/
│   │   ├── socket/          # Socket.IO event handlers (join, edit, leave)
│   │   ├── models/          # MongoDB room/session schemas
│   │   └── routes/          # REST API routes
│   └── Dockerfile
├── docker-compose.yml       # Full-stack orchestration
└── README.md

How It Works

User visits the app and creates or joins a named room
The server creates a MongoDB document for that room and subscribes the user's socket to it
Every edit emits a code-change socket event to the server
The server broadcasts the change to all other sockets in the same room
MongoDB is updated so late-joiners receive the current document state on connection


Screenshots

Add screenshots of the editor, room join screen, and live sync demo here


Author
Pankaj Nehra — github.com/PankajNehra1 | Codeforces: BodaciousLord007
