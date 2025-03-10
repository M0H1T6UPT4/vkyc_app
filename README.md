# Video KYC (VKYC) System

## Overview
The **Video KYC (VKYC) System** is a **real-time video verification platform** that enables agents to verify users via live video sessions. The system leverages **MediaSoup**, and **WebSockets** for seamless and secure communication.

---

## Features
- **ShadCN UI**: Clean and responsive front-end UI.
- **Real-Time Video Streaming**: Powered by WebRTC.
- **WebSocket Signaling**: For real-time session control.
- **Janus Gateway**: Handles media transmission & server-side recording.
- **PostgreSQL + Prisma**: Stores session metadata securely.
- **Agent-Controlled Recording**: Agents can start/stop recording sessions.
- **FFmpeg (Optional)**: Post-processing for recorded video compression & format conversion.

---

## Tech Stack
### **Frontend**
- **Next.js (TypeScript)**
- **ShadCN UI** (Mobile-first user UI, desktop-first agent UI)
- **WebRTC API** (Client-side peer connection)
- **WebSockets** (Real-time signaling)

### **Backend**
- **Next.js API Routes** (App Router)
- **WebSocket Server** (Session control & signaling)
- **Janus Gateway** (WebRTC SFU & recording)
- **PostgreSQL + Prisma** (Database for session storage)
- **FFmpeg (Optional)** (Video processing & compression)

---

## Project Structure
```
/src
  /app
    /api
      /session
        /create/route.ts    # Create VKYC session
        /list/route.ts      # List VKYC sessions
        /end/route.ts       # End VKYC session
  /server
    ws.ts         # WebSocket signaling server
    janus.ts      # Janus API integration
  /lib
    prisma.ts     # Prisma client instance
  /prisma
    schema.prisma # Prisma database schema
  /components
    VideoCall.tsx # WebRTC UI component
  /utils
    helpers.ts    # Utility functions
```

---

## Setup Guide

### Install Dependencies
```sh
pnpm install
```

### Set Up Prisma
```sh
pnpm dlx prisma migrate dev --name init_vkyc_session
pnpm dlx prisma generate
```

### Run Janus Gateway (via Docker)
```sh
./scripts/setup_janus.sh  # Run the automated setup script
```

To manually start Janus:
```sh
docker start janus
```

### Start WebSocket Signaling Server
```sh
pnpm run ws
```

### Start Next.js Development Server
```sh
turbo dev
```

---

## API Endpoints

### **Session Management**
| Method | Endpoint                | Description |
|--------|-------------------------|-------------|
| POST   | `/api/session/create`   | Create a new VKYC session |
| GET    | `/api/session/list`     | Retrieve all VKYC sessions |
| POST   | `/api/session/end`      | End a VKYC session |

---

## WebSocket Events
| Event Type       | Description |
|------------------|-------------|
| `session-request` | User requests a new session |
| `agent-join`      | Agent joins an ongoing session |
| `start-recording` | Agent initiates recording |
| `stop-recording`  | Agent stops recording |
| `session-end`     | Session ends |

---

## Janus Integration
### **Start Recording**
```json
{
  "type": "start-recording",
  "sessionId": "abc123"
}
```

### **Stop Recording**
```json
{
  "type": "stop-recording",
  "sessionId": "abc123"
}
```

---

## Stopping and Cleaning Up
```sh
docker stop janus && docker rm janus
```

---

## Contributors
- **Mohit Gupta** (Master Orchestrator)