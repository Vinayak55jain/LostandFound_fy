# 🔍 Lost & Found — Intelligent Matching System

A smart Lost & Found management platform that helps users report lost or found items and automatically matches them using AI-based image and text analysis, reducing manual effort and response time.

---

## 🚀 Problem Statement

In colleges, offices, public places, and events, lost items are common. Existing systems are often:

- Manual and slow
- Dependent on human coordination
- Inefficient at matching lost and found reports

This project aims to automate matching using image recognition, text similarity, and location-aware logic — with human verification as a fallback.

---

## 🎯 Key Objectives

- Allow users to submit lost and found tickets
- Automatically match lost & found items using AI
- Notify users when a potential match is detected
- Keep a manual resolution flow for unresolved cases
- Reduce resolution time and human dependency

---

## 🧠 System Overview

1) Ticket Creation

- Lost Ticket
  - Name, Roll No, Email, Phone
  - Item description (unique identifiers)
  - Location where item was lost
  - Time (optional)
- Found Ticket
  - Name, Roll No, Email, Phone
  - Item image (photo)
  - Description (optional)
  - Location where item was found

2) Matching Logic (Core Feature)

A hybrid matching approach:

- Image Matching (ML Model)
  - Compare uploaded images with found/lost images using an image-similarity model (e.g., embeddings, siamese network)
- Text Similarity
  - Keyword matching and vector embeddings (TF-IDF or sentence embeddings)
- Location-Based Filtering
  - Prioritize matches within nearby or contextually relevant locations

If a match score crosses a threshold → ticket is marked as a potential match and notifications are sent.

3) Ticket Status Flow

- Matched Tickets
  - Users are notified
  - Ticket marked as Found (or Pending Verification)
- Unresolved Tickets
  - Stored in unresolved queue
  - Can be manually resolved by admins

4) Admin Panel

- View unresolved and flagged tickets
- Manually link lost & found tickets
- Override matching results
- Audit and verification tools

5) Notification System

- When a match is detected both parties receive a notification (email/in-app)
- Match details are shared securely
- Further verification can be completed offline

---

## 🏗️ High-Level Architecture

User → Ticket Submission → Matching Engine (Image + Text + Location) → Ticket Status Update → Notification / Admin Review

Key components:
- Frontend (user forms, admin panel, notifications)
- Backend API (ticket CRUD, matching engine, admin endpoints)
- Database (tickets, users, match history)
- AI Services (image & text similarity)
- Notification service (email/push/in-app)

---

## 🛠️ Tech Stack (Suggested)

- Frontend: React / Next.js, Tailwind CSS
- Backend: Node.js + Express (or NestJS) — REST APIs
- Database: MongoDB or PostgreSQL
- AI / ML: Image similarity model (e.g., ResNet embeddings, CLIP), Text embeddings (e.g., Sentence Transformers)
- Notifications: Email (SMTP / SendGrid), In-app notifications, Optional push
- Deployment: Docker, Kubernetes / Vercel / Heroku

---

## ⚡ Key Features

- Automated lost–found matching
- AI-powered image recognition and text similarity
- Location-aware matching and prioritization
- Manual override for edge cases
- Scalable ticket-based architecture
- Audit logs and match history

---

## 📦 Quick Start (Local)

Prerequisites:
- Node.js >= 16, npm/yarn
- MongoDB / Postgres running
- (Optional) Python & ML environment for models

1. Clone
   git clone https://github.com/Vinayak55jain/LostandFound_fy.git
2. Install (backend)
   cd backend
   npm install
3. Configure
   - Copy `.env.example` to `.env`
   - Set DB_URL, SMTP settings, ML model endpoints, and JWT secrets
4. Run
   npm run dev
5. Frontend
   cd frontend
   npm install
   npm run dev

---

## 🔧 Environment Variables (example)

- DATABASE_URL=mongodb://localhost:27017/lostfound
- PORT=4000
- JWT_SECRET=your_jwt_secret
- SMTP_HOST=
- SMTP_USER=
- SMTP_PASS=
- IMAGE_MODEL_ENDPOINT=http://localhost:5000/embed
- TEXT_MODEL=local|remote

---

## API Endpoints (examples)

- POST /api/tickets/lost — create lost ticket
- POST /api/tickets/found — create found ticket (includes image upload)
- GET /api/tickets/:id — get ticket
- GET /api/tickets/unresolved — admin unresolved list
- POST /api/matches/manual — admin link lost & found tickets
- GET /api/matches/:ticketId — view matches for a ticket

(Implement authentication & rate-limiting for production.)

---

## Matching Algorithm (Details)

- Preprocess:
  - Normalize descriptions, extract keywords, detect item categories
- Image pipeline:
  - Generate embeddings for images (offline or on upload)
  - Compute cosine similarity between embeddings
- Text pipeline:
  - TF-IDF + cosine similarity OR use sentence embeddings
- Location pipeline:
  - Compute proximity score based on locations (e.g., campus zones)
- Final score:
  - Weighted sum of image, text, and location scores
  - If score >= threshold → potential match
- Human-in-the-loop:
  - If confidence below threshold or flagged → add to unresolved queue

Tuning: thresholds and weights are configurable and should be tuned on labeled data.

---

## 📁 Data Model (simplified)

Ticket
- id
- type: lost | found
- user: { name, email, phone, roll_no }
- description
- image_url (found)
- location: { name, lat, lng }
- time_reported
- status: open | matched | resolved
- matched_with: ticketId | null
- match_score: number
- created_at / updated_at

MatchHistory
- id
- ticketA
- ticketB
- score
- verified_by (admin)
- verified_at

---

## 🔐 Security & Privacy

- Validate and sanitize all user inputs
- Store sensitive data securely (no storing of raw passwords — use hashed passwords)
- Access control for admin APIs
- Carefully design image access and sharing to protect user privacy

---

## 📈 Future Enhancements

- Real-time matching using WebSockets
- Mobile app integration
- QR-based found item tagging and retrieval
- Blockchain-based ownership verification for high-value items
- Fraud detection & duplicate prevention
- Multi-language support for descriptions

---

## 🧩 Basic Git & GitHub Guide (Setup & Branching)

A short, practical guide to get contributors started with Git and GitHub for this repo.

### Prerequisites
- Git installed (https://git-scm.com/)
- A GitHub account
- Optional: GitHub CLI (gh) for convenience

### 1) Configure Git (once per machine)

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### 2) SSH key setup (recommended)

Generate a key (if you don't have one):

```bash
ssh-keygen -t ed25519 -C "you@example.com"
# or for older systems:
ssh-keygen -t rsa -b 4096 -C "you@example.com"
```

Add the public key (~/.ssh/id_ed25519.pub) to GitHub: https://github.com/settings/keys
Test the connection:

```bash
ssh -T git@github.com
```

Or use HTTPS if you prefer (you will be prompted for credentials unless you use a credential helper).

### 3) Clone the repo

```bash
git clone git@github.com:Vinayak55jain/LostandFound_fy.git
cd LostandFound_fy
```

### 4) Create a new branch (feature/fix/workflow)

Branch naming suggestions:
- feat/<short-description>
- fix/<short-description>
- docs/<short-description>

```bash
git checkout -b feat/add-match-logging
```

### 5) Make changes & commit

Make your code changes, then:

```bash
git add .
git commit -m "feat: add match logging to matching engine"
```

Write clear, imperative commit messages. Use conventional commits if possible.

### 6) Push your branch

```bash
git push -u origin feat/add-match-logging
```

### 7) Create a Pull Request (PR)

- Open the repository on GitHub and you should see a prompt to create a PR for your pushed branch.
- Or use GitHub CLI:

```bash
gh pr create --fill --title "feat: add match logging" --body "Adds logging for match scoring"
```

Fill in description, link related issues, and set reviewers or assignees.

### 8) Keep your branch up to date

Before merging, sync with main:

```bash
git checkout main
git pull origin main
git checkout feat/add-match-logging
git merge main
# or rebase if preferred:
# git rebase main
```

Resolve any conflicts, run tests, then push the resolved branch.

### 9) Merge & delete branch

- Merge via GitHub UI or with command line after approvals.
- Delete the remote branch once merged:

```bash
git push origin --delete feat/add-match-logging
```

### Useful Commands (quick reference)

```bash
# show status
git status

# view commits
git log --oneline --graph --decorate

# list branches
git branch -a

# switch branches
git checkout main
git checkout -b <branch>

# fetch latest without merging
git fetch origin

# reset local changes (careful)
git checkout -- <file>
```

---

## 🤝 Contributing

Contributions are welcome! Suggested process:
1. Open an issue describing the change/feature
2. Create a branch: feat/your-feature
3. Submit a PR with tests and documentation

Please follow repository coding style and include tests for critical logic (matching engine).

---

## 🧾 License & Credits

- License: MIT (or choose one)
- Maintainer: Vinayak55jain
- This project idea: Lost & Found Intelligent Matching System — combines ML and classic heuristics to reduce manual effort.

---

## Contact

For questions or support, open an issue or contact the maintainer: [Vinayak55jain](https://github.com/Vinayak55jain)
