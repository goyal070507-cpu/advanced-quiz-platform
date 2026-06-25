# ⚡ Real-Time Interactive Quiz Platform (Mentimeter Clone)

A highly performant, real-time multiplayer quizzing application inspired by Mentimeter and Kahoot. Built using a modern TypeScript stack, it allows administrators to create rooms and broadcast questions, while participants join and submit answers in real-time with dynamic, speed-based score calculation.

---

## 📸 Screenshots & Showcase

Here is a visual walk-through of the application in action:

| **Join Room Screen** | **Active Question View** |
|:---:|:---:|
| ![Join Room](images/Screenshot%202023-12-27%20at%2010.18.58%20PM.jpg) | ![Active Question](images/Screenshot%202023-12-27%20at%2010.20.28%20PM.jpg) |

| **Leaderboard Presentation** | **Admin Control Panel** |
|:---:|:---:|
| ![Leaderboard Showcase](images/Screenshot%202023-12-27%20at%2010.20.35%20PM.jpg) | ![Admin Controls](images/Screenshot%202023-12-27%20at%2010.20.46%20PM.jpg) |

---

## ✨ Key Features

### 👤 User / Participant View (`/user`)
- **Seamless Room Entry**: Simply input the Room Code and your Username to immediately enter the live session lobby.
- **Real-Time Answering**: Clean, response-driven MCQ choice buttons that disable upon submission.
- **Speed Bonus Scoring**: Scores are dynamically computed based on response speed:
  $$\text{Points} = 1000 - \left(500 \times \frac{\text{Time Taken}}{\text{Total Question Time}}\right)$$
  Faster correct answers yield higher points!

### 🔑 Admin View (`/admin`)
- **Room Creation**: Instantly instantiate unique Room IDs.
- **Interactive Question Creator**: Add custom MCQs with titles, options, descriptions, and answer configurations on the fly.
- **Quiz Flow Orchestration**: Move sequentially to the next questions or show the Leaderboard to all connected users.

---

## 🛠️ Technology Stack

- **Frontend**: [React.js](https://react.dev/), [Vite](https://vitejs.dev/), [TypeScript](https://www.typescriptlang.org/), [Tailwind CSS](https://tailwindcss.com/)
- **Backend**: [Node.js](https://nodejs.org/), [TypeScript](https://www.typescriptlang.org/), [Socket.io](https://socket.io/) (WebSockets)
- **Communication Protocol**: Bidirectional low-latency events via WebSockets.

---

## 📁 Repository Structure

```tree
├── backend/
│   ├── src/
│   │   ├── managers/
│   │   │   ├── IoManager.ts     # Singleton Socket.io Server wrapper
│   │   │   ├── QuizManager.ts   # Manages active quiz rooms & lifecycle
│   │   │   └── UserManager.ts   # Registers handlers for incoming socket connections
│   │   ├── Quiz.ts              # Core quiz game logic, scoring, and state machine
│   │   └── index.ts             # Entry point (listens on port 3000)
│   ├── package.json
│   └── tsconfig.json
├── frontend/
│   ├── src/
│   │   ├── components/          # Admin, User, Quiz, and Leaderboard components
│   │   ├── App.tsx              # Client router (/admin and /user)
│   │   └── main.tsx
│   ├── package.json
│   └── vite.config.ts
└── README.md
```

---

## 🚀 Getting Started

Follow the instructions below to get a local development copy running.

### 1. Prerequisites
Make sure you have Node.js (v18+) and npm installed.

### 2. Backend Setup
Navigate to the `backend` folder, install the packages, and run the server:
```bash
cd backend
npm install
```

Since the backend is written in TypeScript, you can run it using `ts-node` or compile it:
```bash
# Using ts-node (Recommended for development)
npx ts-node src/index.ts

# Or compile and run with node
npx tsc
node dist/index.js
```
The server will start listening on port `3000`.

### 3. Frontend Setup
Navigate to the `frontend` folder, install dependencies, and start the Vite dev server:
```bash
cd frontend
npm install
npm run dev
```
The dev server will run on `http://localhost:5173`.

> [!NOTE]
> By default, the frontend is configured to connect to a production socket server (`https://sum-server.100xdevs.com`). To run locally, modify the socket initialization in:
> - `frontend/src/components/User.tsx`
> - `frontend/src/components/Admin.tsx`
> 
> Change `io("https://sum-server.100xdevs.com")` to `io("http://localhost:3000")`.

---

## 📡 WebSocket Event Reference

### 1. Client to Server (Emitted Events)

| Role | Event Name | Payload Structure | Description |
|:---|:---|:---|:---|
| **User** | `join` | `{ roomId: string, name: string }` | Joins a quiz room as a participant. |
| **User** | `submit` | `{ userId: string, roomId: string, problemId: string, submission: 0 \| 1 \| 2 \| 3 }` | Submits an MCQ choice selection. |
| **Admin** | `joinAdmin` | `{ password: "ADMIN_PASSWORD" }` | authenticates as admin socket connection. |
| **Admin** | `createQuiz` | `{ roomId: string }` | Creates a new quiz room session. |
| **Admin** | `createProblem` | `{ roomId: string, problem: ProblemObject }` | Adds a question configuration to the active quiz. |
| **Admin** | `next` | `{ roomId: string }` | Manually advances the quiz status to the next question. |

### 2. Server to Client (Received Events)

| Recipient | Event Name | Payload Structure | Description |
|:---|:---|:---|:---|
| **User** | `init` | `{ userId: string, state: CurrentRoomState }` | Sent immediately after joining; returns current quiz state. |
| **User** | `problem` | `{ problem: ProblemObject }` | Broadcasted when a new question is activated. |
| **User** | `leaderboard` | `{ leaderboard: Array<Winner> }` | Broadcasted when admin shows the leaderboard. |

---

## 📜 License
This project is open-source and licensed under the ISC License.