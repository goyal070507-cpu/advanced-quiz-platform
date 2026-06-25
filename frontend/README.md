# 🎨 Quiz Platform — React Frontend

This is the client-side single-page application (SPA) for the real-time interactive quiz platform. It is built using **React**, **Vite**, **TypeScript**, and **Tailwind CSS** to deliver a responsive, highly dynamic, and visually engaging experience for both participants and administrators.

---

## 🧭 Routes & Views

The application relies on `react-router-dom` to route between two primary workspaces:

### 1. 👤 Participant View (`/user`)
- **Location**: `src/components/User.tsx`
- **Purpose**: Enables users to join a live room via a room code and username.
- **Features**:
  - Auto-connects to the backend via WebSockets (`socket.io-client`).
  - Renders the interactive question answering interface.
  - Displays real-time leaderboard rankings as broadcasted by the admin.

### 2. 🔑 Admin View (`/admin`)
- **Location**: `src/components/Admin.tsx`
- **Purpose**: Enables the quiz host to control the session flow.
- **Features**:
  - **Room Manager**: Initialize quiz session rooms.
  - **Question Editor**: Custom creator to compile MCQ titles, description fields, and choices.
  - **Controller Panel**: Triggers the next question broadcast and reveals leaderboard rankings to the room.

---

## 🛠️ Codebase Structure

```tree
src/
├── assets/             # Static visual assets
├── components/         # Modular React views
│   ├── leaderboard/
│   │   └── LeaderBoard.tsx # Dynamic participant ranking board
│   ├── Admin.tsx       # Root Admin Panel
│   ├── User.tsx        # Root Participant view and socket state manager
│   ├── Quiz.tsx        # Active MCQ choice selector screen
│   ├── CreateProblem.tsx   # Admin helper to add questions
│   ├── QuizControls.tsx    # Admin helper to command the session
│   └── CurrentQuestion.tsx # User question display card
├── App.css
├── App.tsx             # Application router definition
├── index.css           # Tailwind CSS directives & global styling
├── main.tsx            # Vite React entry mount point
└── vite-env.d.ts
```

---

## ⚡ Development Setup

To run the frontend client locally:

### 1. Install Dependencies
Make sure you are in the `frontend` directory:
```bash
cd frontend
npm install
```

### 2. Configure Socket Server URI
By default, the client points to a production socket backend. For local testing, open [User.tsx](file:///Users/striker/quiz-app-next/frontend/src/components/User.tsx#L69) and [Admin.tsx](file:///Users/striker/quiz-app-next/frontend/src/components/Admin.tsx#L14) and change the server endpoint:
```typescript
// Replace this:
const socket = io("https://sum-server.100xdevs.com");

// With your local backend address:
const socket = io("http://localhost:3000");
```

### 3. Run Dev Server
Launch Vite's hot-reloading development server:
```bash
npm run dev
```
Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## 🎨 Styling with Tailwind CSS

Tailwind CSS styles the application interfaces utility-first.
- Rules are defined inside `tailwind.config.js`.
- Custom global styles, fonts, and base styling variables reside in [index.css](file:///Users/striker/quiz-app-next/frontend/src/index.css).
- Standard color schemes utilize premium slate, gray, and purple gradients to maximize interactive appeal.

---

## 🛡️ Linting & Strict Typing
- ESLint is configured to enforce clean coding standards (defined in `.eslintrc.cjs`).
- TypeScript definitions are strictly checked using the base `tsconfig.json` configurations.
