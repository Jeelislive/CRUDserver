# Chat App Monorepo

This repository contains a React (Vite) frontend in `client/` and a Node.js/Express backend in `server/`.

- Frontend: Vite + React 18, Material UI, Redux Toolkit
- Backend: Express, Socket.IO, MongoDB (Mongoose)
- Node version: 20.x

## Project Structure

```
chat-app/
├─ client/           # Vite React app
│  ├─ src/
│  ├─ public/
│  ├─ package.json
│  └─ vite.config.js
├─ server/           # Express API + Socket.IO
│  ├─ app.js
│  ├─ routes/
│  ├─ controllers/
│  ├─ models/
│  ├─ middlewares/
│  ├─ utils/
│  └─ package.json
├─ render.yaml       # Render deployment (server)
├─ start.sh          # Helper to start server from root
├─ build.sh          # Helper to build/install server from root
├─ package.json      # Root scripts (server-focused)
└─ README.md         # This file
```

## Prerequisites

- Node.js 20.x
- npm
- MongoDB connection string (for backend)

## Environment Variables

Create and fill `.env` files in both `client/` and `server/`.

- `server/.env` (example):
```
PORT=10000
MONGODB_URI=mongodb://localhost:27017/chatapp
JWT_SECRET=replace_with_strong_secret
CORS_ORIGIN=http://localhost:5173
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
GOOGLE_CLIENT_ID=
```

- `client/.env` (Vite uses VITE_ prefix):
```
VITE_API_URL=http://localhost:10000
VITE_SOCKET_URL=http://localhost:10000
VITE_GOOGLE_CLIENT_ID=
```

Adjust values to your setup.

## Install

Install dependencies separately for client and server.

- Server:
```bash
cd server
npm ci
```

- Client:
```bash
cd client
npm ci
```

Alternatively, from the repo root (server only per root scripts):
```bash
npm install
```
This runs `npm ci` inside `server/`.

## Development

Run server and client in separate terminals.

- Backend (Express):
```bash
cd server
npm run dev
```
Starts on `http://localhost:10000` by default (configurable via `PORT`).

- Frontend (Vite):
```bash
cd client
npm run dev
```
Vite dev server defaults to `http://localhost:5173`.

Ensure CORS/allowed origins in the server `.env` match the client URL.

## Production Builds

- Client production build:
```bash
cd client
npm run build
```
Outputs to `client/dist/`.

- Server production start:
```bash
cd server
npm start
```

Root-level helpers:
```bash
./build.sh   # runs server install (npm ci) from root
./start.sh   # runs server start from root
```

## Deployment

This repo includes `render.yaml` to deploy the backend on Render:

- Build: `cd server && npm ci`
- Start: `cd server && npm start`
- PORT is set to `10000` in `render.yaml`. Render also provides `PORT` in env; prefer using `process.env.PORT` in `server/app.js`.

If deploying the client separately (e.g., Vercel/Netlify):
- Build command: `npm run build`
- Output directory: `dist`
- Set `VITE_API_URL` and `VITE_SOCKET_URL` to your deployed backend URL.

## Scripts Overview

- Root `package.json`:
  - `install`: install server deps
  - `build`: install server deps (for platforms expecting a build step)
  - `start`: start server from root

- `server/package.json`:
  - `start`: `node app.js`
  - `dev`: `nodemon app.js`

- `client/package.json`:
  - `dev`: `vite`
  - `build`: `vite build --mode production`
  - `preview`: `vite preview`

## API and WebSocket

- REST API base URL: `${SERVER_URL}` (e.g., `http://localhost:10000`)
- WebSocket (Socket.IO) URL: same as server URL unless configured otherwise.

Update the client to read from `VITE_API_URL`/`VITE_SOCKET_URL`.

## Troubleshooting

- Check Node version (must be 20.x).
- If client cannot reach server, verify `VITE_API_URL`, `CORS_ORIGIN`, and ports.
- For MongoDB connection issues, validate `MONGODB_URI` and that MongoDB is running.
- On Render, ensure required env vars are configured in the dashboard.

## License

No license provided. Add one if you intend to share or reuse this code.
