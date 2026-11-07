# Vynora Connect — Communication & Notifications Module

A full-stack student networking and collaboration module built with **React + Vite + Tailwind CSS**, **Node.js + Express**, **MongoDB Atlas + Mongoose**, and **Socket.IO**.

---

## 🌟 Architecture & Features

### 1. Frontend (`frontend/`)
- **React 18 + Vite**: Lightning-fast responsive student portal.
- **Tailwind CSS**: Custom student networking design system (`brand-500` royal blue, presence indicators, notification badges).
- **Communication Inbox**:
  - Left pane: Conversation search, filter pills (*All*, *Direct*, *Study Groups*, *Unread*), and *New Chat* modal.
  - Right pane: Active message stream with delivery receipts, student prompts, voice/video sync stubs.
  - Fully responsive: Switches smoothly to full-screen chat on mobile with back navigation.
- **Notification Center**:
  - Filter tabs for *Mentions*, *Messages*, and *System & Deadlines*.
  - Mark single or all notifications as read.
- **Notification Preferences**:
  - Custom toggles for push alerts, campus email digests, and exam quiet hours.

### 2. Backend (`backend/`)
- **Node.js + Express**: RESTful API endpoints with structured controllers and centralized error handling.
- **Mongoose Database Models**:
  - `Conversation`: Direct 1-on-1 and multi-member study groups.
  - `Message`: Chat messages with timestamps, read receipts, and attachment metadata.
  - `Notification`: Student alerts for mentions, new messages, and deadlines.
  - `NotificationSetting`: User delivery preferences and quiet hour schedules.
- **Real-Time Socket.IO**:
  - Room-based real-time broadcasting (`join_conversation`, `send_message`, `new_message`).
  - Real-time notification emission.
  - Presence tracking (online/offline student indicators).
- **Zero-Friction Fallback**:
  - If a MongoDB Atlas URI is not yet configured in `backend/.env`, the server automatically operates in development in-memory mode so APIs and sockets remain fully testable!

---

## 🚀 Quick Start Guide

### 1. Start Both Frontend and Backend

Open two terminal windows:

#### Terminal 1 — Backend Server:
```powershell
cd backend
npm start
```
> Server runs at: **http://localhost:5000**  
> REST API Base: **http://localhost:5000/api**  
> Health check: **http://localhost:5000/api/health**

#### Terminal 2 — Frontend Application:
```powershell
cd frontend
npm run dev
```
> Web Application: **http://localhost:5173**

---

## 🍃 MongoDB Atlas Configuration

When your team provides your MongoDB Atlas cluster connection string:
1. Open `backend/.env`.
2. Add your connection string:
   ```env
   MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.mongodb.net/vynora_connect?retryWrites=true&w=majority
   ```
3. Restart the backend server (`npm start`). Mongoose will automatically connect and persist all messages and notifications.

---

## 📡 REST API Reference

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/health` | Backend status, uptime, and MongoDB connection state |
| `GET` | `/api/conversations` | List all conversations for the student |
| `POST` | `/api/conversations` | Create a new student direct or group chat |
| `GET` | `/api/conversations/:id` | Fetch conversation details |
| `GET` | `/api/conversations/:id/messages` | Fetch message history for a conversation |
| `POST` | `/api/conversations/:id/messages` | Send a new message inside a conversation |
| `GET` | `/api/notifications` | Fetch student notification feed |
| `PATCH` | `/api/notifications/:id/read` | Mark a specific notification as read |
| `PATCH` | `/api/notifications/read-all` | Mark all notifications as read |
| `GET` | `/api/notifications/settings` | Get student notification preferences |
| `PUT` | `/api/notifications/settings` | Update student notification preferences |

---

## ⚡ Real-Time Socket.IO Events

| Event Name | Direction | Payload / Description |
| :--- | :--- | :--- |
| `join_conversation` | Client → Server | `{ conversationId }` — Joins the conversation room |
| `leave_conversation` | Client → Server | `{ conversationId }` — Leaves the conversation room |
| `send_message` | Client → Server | `{ conversationId, text, senderId, senderName }` |
| `new_message` | Server → Client | Broadcasts live message to conversation room members |
| `new_notification` | Server → Client | Broadcasts live alert badge to recipient sockets |
| `presence_update` | Server → Client | Emits online/offline presence changes |
