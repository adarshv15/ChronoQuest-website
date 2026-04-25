# 🎮 ChronoQuest — Time-Trial Puzzle Game

A fast-paced, full-stack **MERN** time-trial game where players race through three progressively challenging rounds against the clock. Complete each stage to unlock the next — only the quickest minds will conquer all three.

---

## 🌟 Overview

ChronoQuest is a browser-based competitive puzzle game built on the MERN stack. It features secure user authentication, persistent progress tracking, a global leaderboard, and a stage-lock progression system — all wrapped in an immersive animated UI.

Players sign up, log in, and work through three distinct game modes:

| Round | Name | Type |
|-------|------|------|
| 🧩 Round 1 | Maze Escape | Navigate a procedural maze to the exit |
| 🧠 Round 2 | Riddle Rush | Answer timed riddles under pressure |
| ⚡ Round 3 | Reaction Clash | Click-based reflex and reaction challenge |

> Round 2 unlocks only after completing Round 1. Round 3 unlocks after Round 2. No shortcuts.

---

## 🚀 Features

- 🌌 **Animated Starfield Homepage** — Immersive space-themed landing page
- 🔐 **User Authentication** — Secure Sign Up & Login with JWT + bcryptjs
- 🗺️ **Game Map Interface** — Visual stage selector showing locked/unlocked rounds
- 🔒 **Stage-Lock Progression** — Linear unlock system enforced on both frontend and backend
- ⏱️ **Per-Round Time Tracking** — Records completion time for each stage
- 🏆 **Global Leaderboard** — Ranked by total completion time across all rounds
- 💾 **Persistent Progress** — Game state saved to MongoDB per user
- 📱 **Responsive Design** — Fully playable on desktop and mobile

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|------------|---------|
| Node.js | Runtime environment |
| Express.js | REST API server |
| MongoDB | Database for users & progress |
| Mongoose | ODM for MongoDB |
| JWT (jsonwebtoken) | Stateless authentication |
| bcryptjs | Password hashing |

### Frontend
| Technology | Purpose |
|------------|---------|
| React.js | Component-based UI |
| React Router DOM | Client-side routing |
| Axios | HTTP requests to backend API |
| CSS3 (Custom) | Animations, gradients, responsive layout |

---

## 📁 Project Structure

```
chronoquest/
│
├── backend/
│   ├── models/
│   │   └── User.js               # User schema — credentials + game progress
│   ├── routes/
│   │   ├── auth.js               # POST /signup, POST /login
│   │   └── game.js               # GET /progress, POST /complete-round, GET /leaderboard
│   ├── middleware/
│   │   └── authMiddleware.js     # JWT verification middleware
│   ├── .env                      # Environment variables
│   ├── server.js                 # Express app entry point
│   └── package.json
│
└── frontend/
    ├── src/
    │   ├── components/
    │   │   ├── HomePage.js       # Animated landing page
    │   │   ├── HomePage.css
    │   │   ├── Login.js          # Login form
    │   │   ├── SignUp.js         # Registration form
    │   │   ├── Auth.css          # Shared auth styles
    │   │   ├── GameMap.js        # Stage selection with lock/unlock UI
    │   │   ├── GameMap.css
    │   │   ├── Round1.js         # Maze Escape game
    │   │   ├── Round2.js         # Riddle Rush game
    │   │   └── Round3.js         # Reaction Clash game
    │   ├── App.js                # Root component with routing
    │   ├── index.js
    │   └── index.css
    └── package.json
```

---

## ⚙️ Installation & Setup

### Prerequisites

- **Node.js** v14+
- **MongoDB** installed and running locally
- **npm** or **yarn**

---

### 1. Clone the Repository

```bash
git clone https://github.com/adarshv15/chronoquest.git
cd chronoquest
```

---

### 2. Install & Start MongoDB

**macOS:**
```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

**Windows / Linux:** Download from [MongoDB Official](https://www.mongodb.com/try/download/community)

---

### 3. Setup Backend

```bash
cd backend
npm install
```

Create a `.env` file in the `backend/` directory:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/chronoquest
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
```

Start the backend server:

```bash
npm run dev
```

Backend runs on **http://localhost:5000**

---

### 4. Setup Frontend

Open a new terminal:

```bash
cd frontend
npm install
npm start
```

Frontend runs on **http://localhost:3000**

---

## 🔑 API Endpoints

### Authentication

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/auth/signup` | Register a new user | ❌ |
| POST | `/api/auth/login` | Login and receive JWT token | ❌ |

### Game Progress

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/game/progress` | Fetch current user's round completion status | ✅ |
| POST | `/api/game/complete-round` | Mark a round as completed, store time | ✅ |
| GET | `/api/game/leaderboard` | Fetch top players ranked by time | ❌ |

---

## 🎮 Game Progression Logic

```
┌─────────────────────────────────────────────┐
│              GAME MAP                        │
│                                             │
│  [Round 1: Maze Escape]  ← Always Unlocked  │
│           │                                 │
│           ▼ (on completion)                 │
│  [Round 2: Riddle Rush]  ← Unlocks          │
│           │                                 │
│           ▼ (on completion)                 │
│  [Round 3: Reaction Clash] ← Unlocks        │
└─────────────────────────────────────────────┘
```

Progress is stored in MongoDB and verified server-side — users cannot skip rounds by manipulating the frontend.

---

## 🔒 Security

- Passwords hashed with **bcryptjs** (salt rounds: 10) — never stored in plaintext
- **JWT tokens** used for stateless session management
- Protected routes verified via middleware on every request
- Tokens stored in `localStorage` with automatic logout on expiry
- CORS configured to allow only frontend origin

---

## 🎨 Design System

| Element | Detail |
|---------|--------|
| Background | Animated starfield (CSS canvas) |
| Color Scheme | Purple `#7C3AED` · Pink `#EC4899` · Blue `#3B82F6` |
| Typography | Clean sans-serif, high contrast |
| Animations | Smooth CSS transitions, keyframe star movement |
| States | Locked (dimmed + 🔒 icon) / Unlocked (glowing + active) |

---

## 🐛 Troubleshooting

**MongoDB connection error:**
```bash
brew services start mongodb-community   # macOS
# or check if mongod is running on port 27017
```

**Port already in use:**
```bash
# Kill backend port
lsof -ti:5000 | xargs kill

# Kill frontend port
lsof -ti:3000 | xargs kill
```

**CORS errors:**
- Ensure backend `.env` has the correct `PORT`
- Frontend Axios base URL must point to `http://localhost:5000`

---

## 📸 Screenshots

> *(Add your screenshots here)*

| Homepage | Game Map |
|---|---|
| ![Home](screenshots/home.png) | ![Map](screenshots/gamemap.png) |

| Round 1 — Maze | Leaderboard |
|---|---|
| ![Maze](screenshots/maze.png) | ![Leaderboard](screenshots/leaderboard.png) |

---

## 🔮 Roadmap

- [x] User authentication (JWT + bcrypt)
- [x] Game map with stage-lock system
- [x] Backend progress API + leaderboard endpoint
- [x] Animated homepage
- [ ] Round 1: Maze Escape — Canvas-based maze renderer
- [ ] Round 2: Riddle Rush — Timer + Q&A engine
- [ ] Round 3: Reaction Clash — Click reflex game
- [ ] Leaderboard UI with rankings
- [ ] Sound effects & game over screens
- [ ] Docker deployment setup

---

## 👨‍💻 Author

**Adarsha V**
- GitHub: [@adarshv15](https://github.com/adarshv15)
- Email: adarsh80737@gmail.com

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

*Built with ❤️ using the MERN Stack — MongoDB · Express · React · Node.js*
