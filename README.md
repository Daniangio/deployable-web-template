# Deployable Web Game Template

This repository is a game-unspecific browser game services template. It provides
the reusable backbone around a game: Firebase auth, player profiles, friends,
presence, global/group/direct chat, and a small admin backoffice.

It intentionally does not create game rooms or implement game rules. Add those
inside your game-specific layer.

## Stack

- Backend: FastAPI, SQLAlchemy, PostgreSQL, Redis, Firebase Admin SDK.
- Frontend: React, Vite, Firebase Web SDK, Zustand.
- Realtime: WebSocket gateway for presence and chat.
- Deployment: Docker Compose for local development and production.

## Quick Start

1. Copy the env template:

   ```bash
   cp .env.example .env
   ```

2. Fill in Firebase Web SDK values and the backend Firebase project/admin email
   in `.env`.

3. Put the Firebase Admin SDK JSON file at the path configured by
   `FIREBASE_ADMIN_CREDENTIALS`, for example:

   ```text
   secrets/firebase-admin.dev.json
   ```

4. Run the stack:

   ```bash
   docker compose up --build
   ```

5. Open:

   - Frontend: http://localhost:5173
   - Backend API: http://localhost:8000
   - Backend docs: http://localhost:8000/docs
   - Adminer: http://localhost:8081

## Firebase Setup

Create or select a Firebase project, then enable email/password sign-in:

1. In the Firebase Console, open **Authentication**.
2. Go to **Sign-in method**.
3. Enable **Email/Password**.

Configure the frontend Firebase Web SDK:

1. In the Firebase Console, open **Project settings**.
2. Under **Your apps**, create or select a Web app.
3. Copy the Firebase config values into `.env`:

   ```text
   VITE_FIREBASE_API_KEY=...
   VITE_FIREBASE_AUTH_DOMAIN=...
   VITE_FIREBASE_PROJECT_ID=...
   VITE_FIREBASE_APP_ID=...
   VITE_FIREBASE_MESSAGING_SENDER_ID=...
   VITE_FIREBASE_STORAGE_BUCKET=...
   ```

The frontend Firebase wrapper is committed at
`frontend/src/lib/firebase.js`. Do not create this file from secrets and do not
ignore `frontend/src/lib/`; a fresh repo created from this template must include
that source file.

Configure the backend Firebase Admin SDK:

1. In the Firebase Console, open **Project settings**.
2. Go to **Service accounts**.
3. Generate a new private key JSON file.
4. Save it under `secrets/`, for example:

   ```text
   secrets/firebase-admin.dev.json
   ```

5. Point `.env` at that file and project:

   ```text
   FIREBASE_ADMIN_CREDENTIALS=secrets/firebase-admin.dev.json
   FIREBASE_PROJECT_ID=your-firebase-project-id
   FIREBASE_PRIMARY_ADMIN_EMAIL=admin@example.com
   ```

`FIREBASE_PRIMARY_ADMIN_EMAIL` is optional, but useful for local development:
when that Firebase user first signs in, the backend promotes the account to
admin automatically.

## Configuration

All local runtime configuration is in `.env`. Use `.env.example` as the source
of truth for ports, Firebase, Redis, Postgres, chat retention, and backend
session settings.

Secret files under `secrets/` are intentionally ignored by git.

## Template Checklist

When creating a new repo from this template, verify these files exist before
running Vite:

```text
frontend/src/lib/firebase.js
frontend/src/app/App.jsx
.env.example
```

If Vite reports:

```text
Failed to resolve import "../lib/firebase.js" from "src/app/App.jsx"
```

then `frontend/src/lib/firebase.js` was not copied or was ignored by git. Restore
it from this template and check that `.gitignore` does not contain a broad
`lib/` rule that also matches `frontend/src/lib/`.

## Validation

```bash
python -m pytest -q
cd frontend && npm install && npm run build
```
