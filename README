# Rewer.ai — AI-Powered Competitive Coding Platform

> A full-stack, LeetCode-style competitive programming platform enhanced with a **Multi-Agent AI system** for real-time code assistance, debugging, and mentorship.

##  Project Overview

**Rewer.ai** is an intelligent online judge platform where users can:
- Submit code that gets securely executed via **Judge0**
- Receive **AI-assisted help** from a multi-agent system (hints, debug support, code review)
- Track their progress, star problems, and engage with the community via comments/solutions

---

##  Core Features

### Authentication
- Sign up / Login via **Firebase Auth**
- JWT session management for API calls

###  Problem Management
- Problem list with filters: difficulty (Easy / Medium / Hard), tags (DP, Graph, Greedy, etc.), status (solved / unsolved / attempted)
- Each problem has:
  - Title, Description, Constraints, Examples
  - Hidden & visible test cases
  - Editorial / Official Solution (unlockable)
  - Community solutions
  - Like / Dislike system
  - Comment thread

###  Code Execution (Judge0)
- language support (Python, C++, Java)
- Real-time execution feedback: stdout, stderr, time, memory
- Test against sample cases before full submission
- Rate limiting on submissions to prevent abuse

###  AI Agent System
- Hint generator (without giving away the full solution)
- Step-by-step debugger (analyzes wrong output and traces the bug)
- Code reviewer (time/space complexity analysis, style, edge case suggestions)
- Chatbot interface embedded in the problem view

###  User Dashboard
- Submission history with verdict (Accepted, WA, TLE, MLE, RE, CE)
- Heatmap of daily submissions (solving graph)
- Stats: problems solved by difficulty, language breakdown, acceptance rate

###  Starred Problems & Solutions
- Users can star problems for later
- Successful submissions are stored and can be marked as public/private solution is availble
- "My Solutions" page with filtering

---

##  Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| **React + Vite** | Fast SPA bundling with HMR |
| **React Router** | Client-side routing |
| **Tailwind CSS** |  styling |
| **Firebase Auth** | Authentication UI & session management |
| **Monaco Editor** | VS Code-like in-browser code editor |
| **Zustand** | Lightweight global state management |
| **Framer Motion(Maybe ??)** | UI animations |
| **React Hot Toast** | Notification toasts |

### Backend
| Technology | Purpose |
|---|---|
| **Flask** | REST API server |
| **Pydantic** | Request/response validation & schema enforcement |
| **LangChain** | LLM pipeline & chains |
| **LangGraph** | Multi-agent orchestration (stateful agent graphs) |
| **Judge0 Python SDK** | Secure code execution |
| **Flask-Limiter** | Rate limiting on submission endpoints |
| **Celery + Redis(MayBe required)** | Async task queue for code submission jobs |
| **PyMongo** | MongoDB driver (sync / async) |

### Database
| Technology | Purpose |
|---|---|
| **MongoDB** | Primary database (problems, submissions, users) |
| **Redis** | Caching + rate limiting + Celery broker |

### Infrastructure / DevOps
| Technology | Purpose |
|---|---|
| **Docker + Docker Compose** | Containerized services |
| **Judge0 (self-hosted or cloud)** | Sandboxed code execution |
| **Nginx** | Reverse proxy |

---

##  System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                        CLIENT                           │
│   React + Vite + Tailwind + Monaco Editor               │
│   Clerk Auth → JWT Token                               │
└─────────────────────┬───────────────────────────────────┘
                      │ HTTPS REST API
┌─────────────────────▼───────────────────────────────────┐
│                   FLASK BACKEND                         │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  Auth MW     │  │  Submission  │  │  AI Agent    │  │
│  │  (Clerk JWT) │  │  Controller  │  │  Controller  │  │
│  └──────────────┘  └──────┬───────┘  └──────┬───────┘  │
│                           │                 │           │
│                    ┌──────▼──────┐   ┌──────▼──────┐   │
│                    │  Celery     │   │  LangGraph  │   │
│                    │  Task Queue │   │  Agents     │   │
│                    └──────┬──────┘   └─────────────┘   │
│                           │                             │
│                    ┌──────▼──────┐                      │
│                    │   Judge0    │                      │
│                    │  Execution  │                      │
│                    └─────────────┘                      │
└──────────────┬──────────────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────────────┐
│                     DATA LAYER                          │
│         MongoDB                      Redis              │
│  (Problems, Users, Submissions)  (Cache + Rate Limit)   │
└─────────────────────────────────────────────────────────┘
```

---

##  Database Schema

### Collection: `problems`
```json
{
  "_id": "ObjectId",
  "title": "Two Sum",
  "slug": "two-sum",
  "description": "...",
  "difficulty": "Easy",
  "tags": ["Array", "Hash Table"],
  "constraints": "...",
  "examples": [
    { "input": "[2,7,11,15], target=9", "output": "[0,1]", "explanation": "..." }
  ],
  "test_cases": [
    { "input": "...", "expected_output": "...", "is_hidden": true }
  ],
  "starter_code": { "python": "...", "cpp": "..." },
  "editorial": "...",
  "likes": 1024,
  "dislikes": 42,
  "total_submissions": 5000,
  "successful_submissions": 3200,
  "acceptance_rate": 64.0,
  "created_at": "ISODate",
  "created_by": "admin"
}
```

### Collection: `submissions`
```json
{
  "_id": "ObjectId",
  "user_id": "ObjectId",
  "problem_id": "ObjectId",
  "language": "python",
  "code": "def twoSum(nums, target): ...",
  "verdict": "Accepted",
  "runtime_ms": 48,
  "memory_kb": 14200,
  "test_cases_passed": 72,
  "total_test_cases": 72,
  "submitted_at": "ISODate",
  "judge0_token": "abc-xyz"
}
```

### Collection: `users`
```json
{
  "_id": "ObjectId",
  "clerk_id": "user_xxxxx",
  "username": "devuser123",
  "email": "user@example.com",
  "avatar_url": "...",
  "bio": "...",
  "github": "https://github.com/...",
  "starred_problems": ["ObjectId", "ObjectId"],
  "solved_problems": ["ObjectId"],
  "attempted_problems": ["ObjectId"],
  "streak": 7,
  "joined_at": "ISODate",
  "last_active": "ISODate"
}
```

### Collection: `comments`
```json
{
  "_id": "ObjectId",
  "problem_id": "ObjectId",
  "user_id": "ObjectId",
  "content": "Great problem! Here's a hint...",
  "parent_id": null,
  "upvotes": 34,
  "created_at": "ISODate"
}
```

### Collection: `user_solutions`
```json
{
  "_id": "ObjectId",
  "user_id": "ObjectId",
  "problem_id": "ObjectId",
  "submission_id": "ObjectId",
  "title": "My O(n) approach",
  "explanation": "Using a hashmap to...",
  "is_public": true,
  "upvotes": 12,
  "created_at": "ISODate"
}
```

---

##  Multi-Agent AI System

Built using **LangGraph** for stateful, graph-based agent orchestration.

### Agent Roles

#### 1.  Orchestrator Agent
- Entry point that receives user query
- Routes to the correct specialist agent
- Manages conversation context and memory

#### 2.  Hint Agent
- Receives: problem description + user's current code + error (if any)
- Produces: Socratic, progressive hints (3 levels: concept → approach → near-solution)
- Does NOT reveal the direct solution

#### 3.  Debugger Agent
- Receives: code + test case input + actual output + expected output
- Traces the logical error step-by-step
- Suggests fixes without rewriting the code entirely

#### 4.  Code Review Agent
- Analyzes submitted/accepted code for:
  - Time and space complexity
  - Code style & readability
  - Edge case handling
  - Alternative, more optimal approaches

#### 5. Chat Agent (General Q&A)
- Explains data structures, algorithms, and concepts
- Answers follow-up questions in context of the current problem

### LangGraph Flow
```
User Query
    │
    ▼
Orchestrator Agent
    ├──► Hint Agent         → Structured hints
    ├──► Debugger Agent     → Bug explanation + fix suggestion  
    ├──► Code Review Agent  → Complexity + improvements
    └──► Chat Agent         → General explanation
```

### Memory & Context
- Short-term memory: Conversation history stored per session (in-memory or Redis)
- Problem context injected as system prompt for every agent call
- Agents share a LangGraph `State` object containing: `problem`, `user_code`, `error_trace`, `chat_history`

---

##  Backend Design

### API Endpoints (Flask)

#### Auth
- `POST /api/auth/verify` — Verify Clerk JWT, create/update user in MongoDB

#### Problems
- `GET /api/problems` — Paginated list with filters
- `GET /api/problems/:slug` — Problem detail + starter code
- `POST /api/problems` — Admin: create problem (Pydantic validated)
- `PUT /api/problems/:id` — Admin: update problem
- `POST /api/problems/:id/like` — Like / unlike

#### Submissions
- `POST /api/submit` — Submit code (rate-limited, queued via Celery)
- `GET /api/submit/:token` — Poll submission result
- `POST /api/run` — Run against sample cases (not stored)
- `GET /api/submissions/:problem_id` — User's past submissions on a problem

#### AI Agent
- `POST /api/agent/chat` — Send message to multi-agent system
- `GET /api/agent/session/:id` — Retrieve chat history

#### User
- `GET /api/users/:username` — Public profile
- `GET /api/users/me` — Private profile + stats
- `POST /api/users/star/:problem_id` — Star/unstar a problem

### Rate Limiting Strategy (Flask-Limiter)
```python
@app.route("/api/submit", methods=["POST"])
@limiter.limit("5 per minute")
@limiter.limit("30 per hour")
def submit_code():
    ...
```

---

##  Frontend Design

### Pages & Routes

| Route | Page |
|---|---|
| `/` | Landing page |
| `/problems` | Problem list |
| `/problems/:slug` | Problem detail + code editor |
| `/profile/:username` | User profile |
| `/dashboard` | Personal stats & history |
| `/leaderboard` | Global rankings |
| `/admin` | Problem management (admin only) |

### Key UI Components

- **Monaco Editor** — Multi-language code editor with syntax highlighting
- **Split-pane layout** — Problem description (left) + Editor + Console (right)
- **AI Chat Drawer** — Slide-in chat panel powered by the multi-agent system
- **Verdict Toast** — Animated feedback on submission result
- **Test Case Panel** — Run against custom inputs before submitting
- **Submission History Table** — Filterable, sortable past submissions

---

##  Additional Features to Implement

These are meaningful additions that fit naturally with the current architecture:

###  Leaderboard & Gamification
- Global and weekly leaderboard ranked by problems solved and acceptance rate
- Badges and achievements (e.g., "Solved 50 Hard Problems", "7-day Streak")
- XP/points system per submission difficulty

### Problem Sets / Contests
- Admin creates time-limited contests (like Codeforces/LeetCode Weekly)
- Contest leaderboard calculated in real-time
- Problems unlocked sequentially during a contest

###  Admin Panel
- Rich UI for creating/editing problems with test case management
- Preview how a problem renders to users
- Bulk import problems via JSON/CSV

###  Multi-Language Starter Code Templates
- Auto-generate starter code per language from a single function signature definition
- Reduce admin burden of writing starters for every language

### Analytics Dashboard (Admin)
- Track which problems are too hard/easy (low/high acceptance)
- Monitor platform usage, submissions per day, popular languages

###  Threaded Discussions per Problem
- Nested comment system (like LeetCode's Discuss tab)
- Markdown support in comments
- Upvote/downvote comments



###  Custom Test Case Runner
- Users can enter their own inputs to test edge cases before submission
- Powered by Judge0 run (not stored)


---

## 🚀 Improvements & Enhancements

These are improvements to the current plan that will significantly raise quality:


### 1. 🧠 Agent Memory with Redis
**Problem:** Each AI call is stateless — agents lose context between messages.  
**Improvement:** Store conversation history in Redis with a session key. Pass full history to LangGraph on each turn.  
**Benefit:** Coherent, multi-turn debugging conversations.

### 5. 🗃 MongoDB Indexing Strategy
**Problem:** Without indexes, problem list queries slow down as data grows.  
**Improvement:** Add compound indexes on `{ difficulty, tags, slug }` for problems, `{ user_id, problem_id }` for submissions.  
**Benefit:** Sub-10ms query times even at scale.

### 6. 🔑 Role-Based Access Control (RBAC)
**Problem:** Currently no distinction between admin and regular user at the API level.  
**Improvement:** Add `role` field to user document (`"user"`, `"admin"`). Middleware checks role on protected routes.  
**Benefit:** Secure admin panel, problem creation locked to admins.

### 8. 🧹 Code Execution Sandboxing Review
**Problem:** Judge0 is the execution layer, but misconfigured self-hosting can expose security risks.  
**Improvement:** Use Judge0 Cloud or ensure self-hosted Judge0 runs in isolated Docker containers with CPU/memory/time limits enforced.  
**Benefit:** Prevents resource abuse and code injection attacks.

### 9. 🎯 AI Agent — Guardrails & Prompt Engineering
**Problem:** Without guardrails, the AI agent might give away full solutions.  
**Improvement:** Add a `SolutionGuard` validation step in LangGraph that checks if the agent response contains direct code answers, and rewrites it as a hint.  
**Benefit:** Preserves learning value while still being helpful.

### 10. 📊 User Stats Caching
**Problem:** Computing "problems solved by difficulty" on every profile visit is expensive.  
**Improvement:** Pre-compute and cache stats in Redis on each successful submission. Invalidate on new submission.  
**Benefit:** Instant profile load, reduced DB load.

---

## 🔒 Security Considerations

| Risk | Mitigation |
|---|---|
| Code injection via submissions | Judge0 sandboxed execution, no direct `eval` |
| JWT tampering | Verify Clerk JWT signature server-side on every request |
| Brute force submissions | Flask-Limiter rate limiting (5/min, 30/hour) |
| XSS in comments | Sanitize markdown with `DOMPurify` on frontend |
| Admin route abuse | RBAC middleware on all admin endpoints |
| MongoDB injection | Pydantic validation + PyMongo parameterized queries |
| API scraping | Rate limit all public endpoints |

---

## 🚢 Deployment Strategy

```
GitHub → GitHub Actions CI/CD
    │
    ├── Run tests (pytest, vitest)
    ├── Build Docker images
    └── Push to registry
         │
         ▼
    Docker Compose (VPS / Cloud)
    ├── flask-api (Flask + Gunicorn)
    ├── celery-worker (Celery)
    ├── redis (Rate limit + Cache + Broker)
    ├── mongodb (Database)
    ├── judge0 (Self-hosted or external)
    └── nginx (Reverse proxy + SSL)
```

### Recommended Hosting
- **VPS:** DigitalOcean Droplet or Hetzner (good price/performance)
- **MongoDB:** MongoDB Atlas (free tier for dev, M10 for production)
- **Judge0:** Use [Judge0 Cloud](https://judge0.com) for simplicity, or self-host on a separate server
- **Frontend:** Vercel or Cloudflare Pages (static React build)
