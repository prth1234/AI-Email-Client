<p align="center">
  <img src="docs/assets/hero.svg" alt="AI Email Client hero banner" width="100%" />
</p>

<p align="center">
  <a href="#overview">Overview</a> &middot;
  <a href="#features">Features</a> &middot;
  <a href="#architecture">Architecture</a> &middot;
  <a href="#setup--installation">Setup</a> &middot;
  <a href="#api-endpoints">API Endpoints</a>
</p>

---

## Overview

**AI Email Client** is a full-stack Gmail reader built with a **React 18 + GitHub Primer (`@primer/react`)** frontend and an **Express + `googleapis`** backend. It authenticates via Google OAuth 2.0, fetches paginated inbox threads, extracts HTML and plain-text MIME payloads, and renders a split-pane reading workspace with a resizable sidebar and persistent dark/light theme.

---

## Features

<p align="center">
  <img src="docs/assets/features.svg" alt="AI Email Client key features" width="100%" />
</p>

- **Google OAuth 2.0 Flow**: Authenticates against Gmail API (`gmail.readonly`, `gmail.modify`, `userinfo.email`).
- **Resizable Split-Pane Workspace**: Drag-to-resize sidebar (`280px` to `800px`) persisted in `localStorage`.
- **Keyboard-First Search**: Press `/` anywhere outside inputs to focus the search bar.
- **Dark & Light Primer Modes**: Toggle between Primer dark (`#0d1117` / `#000000`) and light (`#f6f8fa` / `#ffffff`) themes.
- **Recursive MIME Parsing**: Decodes nested `text/html` and `text/plain` message parts and strips HTML entities for clean previews.

---

## Architecture

<p align="center">
  <img src="docs/assets/architecture.svg" alt="Full-stack OAuth and Gmail API flow" width="100%" />
</p>

<details>
<summary><strong>Tech Stack Breakdown</strong></summary>

| Layer | Technology |
|---|---|
| Frontend | React 18, Vite 7, `@primer/react` 36, `@primer/octicons-react`, `styled-components` |
| Backend | Node.js, Express 4, `googleapis` 126, `cors`, `dotenv` |
| Auth | Google OAuth 2.0 Web Application flow |

</details>

---

## Setup & Installation

### Quick Start

```bash
chmod +x setup.sh start.sh
./setup.sh
./start.sh dev
```

Open `http://localhost:5173`, sign in with your Google test-user account, and browse your inbox.

<details>
<summary><strong>Prerequisites (Google Cloud &amp; OAuth 2.0 Setup)</strong></summary>

1. **Node.js &amp; npm** installed locally.
2. **Google Cloud Project** with the **Gmail API** enabled:
   - Go to [Google Cloud Console](https://console.cloud.google.com/) and create a project.
   - Under **APIs &amp; Services &gt; Library**, enable **Gmail API**.
   - Under **APIs &amp; Services &gt; OAuth consent screen**, select **External**, fill in app details, and add your Gmail address under **Test users**.
   - Under **APIs &amp; Services &gt; Credentials**, create an **OAuth client ID** (type: **Web application**) with authorized redirect URI:
     ```text
     http://localhost:3001/oauth2callback
     ```
   - Copy your **Client ID** and **Client Secret**.

</details>

<details>
<summary><strong>Environment Variables (<code>backend/.env</code>) &amp; Manual Run</strong></summary>

Create `backend/.env` with your Google OAuth credentials:

```env
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
REDIRECT_URI=http://localhost:3001/oauth2callback
PORT=3001
```

Install dependencies manually if not using `./setup.sh`:

```bash
cd backend && npm install
cd ../frontend && npm install
```

Start each server separately:

```bash
# Terminal 1 - Backend (http://localhost:3001)
cd backend && npm start

# Terminal 2 - Frontend (http://localhost:5173)
cd frontend && npm run dev
```

</details>

---

## API Endpoints

<details open>
<summary><strong>Express Backend Routes (<code>http://localhost:3001</code>)</strong></summary>

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/auth/url` | Returns the Google OAuth 2.0 consent URL |
| `GET` | `/auth/google` | Redirects to Google OAuth 2.0 sign-in |
| `GET` | `/oauth2callback` | Handles OAuth callback and redirects to `http://localhost:5173/` |
| `GET` | `/api/gmail/profile` | Checks authentication and returns the user's Gmail profile |
| `GET` | `/api/emails` | Lists recent emails with decoded headers and snippets |
| `GET` | `/api/emails/:id` | Fetches full decoded HTML and plain-text body for a message |
| `POST` | `/api/emails/:id/read` | Marks a message as read |

</details>

---

## Project Structure

<details>
<summary><strong>Repository File Tree</strong></summary>

```text
AI-Email-Client/
├── SETUP_INSTRUCTIONS.md
├── setup.sh                      # Installs backend & frontend dependencies
├── start.sh                      # Starts dev servers (./start.sh dev)
├── backend/
│   ├── package.json
│   └── server.js                 # Express + googleapis OAuth & Gmail API server
└── frontend/
    ├── package.json
    ├── vite.config.js
    └── src/
        ├── App.jsx               # Main workspace, search shortcut, theme & resize state
        ├── main.jsx
        └── components/
            ├── LoginView.jsx     # OAuth sign-in screen
            ├── Sidebar.jsx       # Resizable navigation & inbox header
            ├── EmailList.jsx     # Message list
            └── EmailDetail.jsx   # Message body viewer
```

</details>
