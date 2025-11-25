# Data Flow & Process Analysis

## 1. Overview
This document details the flow of data within the Mental Health Companion application, tracing the lifecycle of information from user input through processing, storage, and retrieval. It highlights the interaction between the frontend, backend API, AI services, and the database.

## 2. Core Data Flows

### 2.1 User Authentication & Session Management
*   **Input**: User credentials (email/password) via Login Page.
*   **Process**:
    1.  Frontend sends credentials to Supabase Auth.
    2.  Supabase validates credentials and returns a JWT (JSON Web Token).
    3.  **Trigger**: On successful signup, a PostgreSQL trigger (`on_auth_user_created`) automatically inserts a record into the `public.profiles` table.
*   **Output**: Authenticated session state in Frontend; User Profile data in Database.

### 2.2 AI-Driven Therapy Session
This flow represents the core value proposition of the application.

1.  **User Input**: User types a message in the `Chat` interface.
2.  **API Request**: Frontend sends `POST /chat/message` to Backend with the message content and history context.
3.  **AI Processing**:
    *   `chat.py` router invokes `ai_service.py`.
    *   **Emotion Analysis**: The text is sent to Gemini with a specific prompt to extract `primary_emotion`, `intensity`, and `sentiment`.
    *   **Crisis Detection**: Parallel request to Gemini to check for "CRISIS" or "SAFE" flags.
    *   **Response Generation**: The conversation history and new prompt are sent to Gemini to generate a therapeutic response.
4.  **Data Persistence**:
    *   The User's message and the AI's response are stored in the `chats` table.
    *   Metadata (emotion, crisis status) is stored alongside the message.
5.  **Response**: The backend returns the AI response and analysis data to the Frontend for display.

### 2.3 Mood Tracking & Symphony
1.  **Input**: User selects a mood score (1-10) and adds a note in the `Journal` or `Dashboard`.
2.  **Processing**:
    *   Data is validated by `journal.py` or `wellness.py`.
    *   (Optional) AI analysis of the note to infer implicit emotion.
3.  **Storage**: Record inserted into `mood_logs` table.
4.  **Real-time Connection (Symphony)**:
    *   **Input**: User joins "The Lighthouse" (Symphony page).
    *   **Process**:
        *   Frontend establishes WebSocket connection to `/symphony/ws`.
        *   Backend (`symphony.py`) assigns anonymous ID and broadcasts user's position/mood-color to all connected clients.
    *   **Interaction**: User clicks a star to send a "Pulse". Backend relays message to target client via WebSocket.

### 2.4 Mind Garden (Gamification)
1.  **Trigger**: User completes an activity (Meditation, Focus Session, Journal Entry).
2.  **Process**:
    *   Frontend sends completion data to respective endpoint (e.g., `/focus/session`).
    *   Backend calculates XP reward (e.g., +1 XP/min for Focus).
    *   `garden_service.py` updates `mind_garden` table for the specific plant type (Oak, Lotus, etc.).
3.  **Output**: Updated XP and Plant Level returned to frontend; Plant SVG grows visually.

### 2.4 Therapist-Client Data Sync
1.  **Access**: Therapist logs into `TherapistDashboard`.
2.  **Query**: Frontend requests `/therapist/clients/{id}`.
3.  **Authorization**: Backend verifies the Therapist's role and their relationship to the client in `therapist_clients` table.
4.  **Retrieval**:
    *   Backend executes a join query on `profiles`, `mood_logs`, and `journal_entries`.
    *   Sensitive data is filtered based on RLS policies.
5.  **Output**: Aggregated view of Client's recent moods, journal summaries, and crisis alerts.

## 3. Database Schema & Relationships

### 3.1 Entity-Relationship Overview
*   **Profiles (`profiles`)**: Central entity. Linked 1:1 with Supabase Auth Users.
*   **Chats (`chats`)**: N:1 relationship with Profiles. Stores conversation history.
*   **Journal (`journal_entries`)**: N:1 relationship with Profiles. Stores unstructured reflections.
*   **Mood Logs (`mood_logs`)**: N:1 relationship with Profiles. Stores quantitative data.
*   **Therapist-Client (`therapist_clients`)**: N:N relationship linking two Profiles (one 'therapist', one 'user').

### 3.2 Security Model (Row Level Security)
*   **Self-Access**: Policies like `Users can CRUD their own data` ensure strict privacy.
*   **Therapist Access**: Specific policies allow therapists to `SELECT` data from users linked in `therapist_clients`.

## 4. API Endpoint Summary

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/auth/*` | Managed by Supabase Client directly |
| `POST` | `/chat/send` | Process user message and return AI response |
| `GET` | `/journal/entries` | Retrieve user's past journal entries |
| `POST` | `/journal/create` | Create a new journal entry with mood tag |
| `GET` | `/therapist/clients` | List all clients for a logged-in therapist |
| `WS` | `/symphony/ws` | Real-time WebSocket connection for Symphony |
| `POST` | `/focus/session` | Log focus session and award XP |
| `GET` | `/garden/{id}` | Fetch user's Mind Garden state |
