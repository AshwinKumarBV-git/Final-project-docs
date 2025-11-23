# System Architecture

## 1. Executive Summary

The **Mental Health Companion** is a comprehensive digital health platform designed to provide accessible, AI-driven mental health support. The system integrates real-time conversational AI, mood tracking, journaling, and professional therapist oversight into a unified web application. This document outlines the high-level architecture, component breakdown, and technical stack used to deliver these services.

## 2. High-Level Architecture

The system follows a modern **Client-Server** architecture with a decoupled frontend and backend, supported by a cloud-native database and external AI services.

### Architectural Diagram Description
1.  **Client Layer (Frontend)**: A Single Page Application (SPA) built with React and Vite, serving as the user interface.
2.  **API Layer (Backend)**: A RESTful API built with FastAPI (Python), handling business logic, authentication, and data orchestration.
3.  **Data Layer (Database)**: Supabase (PostgreSQL) serves as the primary data store, utilizing Row Level Security (RLS) for robust data protection.
4.  **Intelligence Layer (AI)**: Google Gemini 2.5 Flash provides the core AI capabilities for natural language understanding, emotion analysis, and crisis detection.

## 3. Component Breakdown

### 3.1 Frontend (Client Layer)
The frontend is designed for responsiveness and interactivity, utilizing a component-based architecture.

*   **Framework**: React 19 (via Vite)
*   **Routing**: React Router DOM v7
*   **Styling**: TailwindCSS v4 with PostCSS
*   **UI Components**: Custom components with Framer Motion for animations and Lucide React for iconography.
*   **State Management**: React Hooks (useState, useEffect, useContext).
*   **Key Modules**:
    *   **Dashboard**: Central hub for user wellness metrics.
    *   **Chat Interface**: Real-time communication with the AI therapist.
    *   **Therapy & Meditation**: Guided sessions and audio-visual tools.
    *   **Games**: Interactive cognitive exercises (e.g., Breathing Game).
    *   **Therapist Dashboard**: specialized view for clinicians to monitor client progress.

### 3.2 Backend (API Layer)
The backend is a high-performance asynchronous web server.

*   **Framework**: FastAPI
*   **Server**: Uvicorn
*   **Structure**: Modularized into `routers` (endpoints), `services` (logic), and `core` (config).
*   **Key Modules**:
    *   `auth.py`: Handles user authentication and session management.
    *   `chat.py` & `therapy.py`: Manages conversational flows and session contexts.
    *   `therapist.py`: Provides endpoints for clinician access to client data.
    *   `ai_service.py`: Interface for Google Gemini API interactions.
    *   `wellness.py`: Manages wellness plans and progress tracking.

### 3.3 Data Layer (Database)
The system leverages a relational database with strict security policies.

*   **Platform**: Supabase (PostgreSQL)
*   **Key Tables**:
    *   `profiles`: User identity and roles (user/therapist).
    *   `chats`: History of conversations with metadata (emotion, crisis flags).
    *   `journal_entries`: User-generated content with mood tags.
    *   `mood_logs`: Quantitative mood scores for tracking.
    *   `wellness_plans`: JSON-based structured plans for users.
*   **Security**: Row Level Security (RLS) policies ensure users can only access their own data, while therapists have scoped access to assigned clients.

### 3.4 External Services
*   **Google Gemini API**:
    *   **Model**: `gemini-2.5-flash`
    *   **Function**: Generates therapeutic responses, analyzes sentiment, and detects crisis patterns.
*   **Supabase Auth**: Handles JWT-based authentication and identity management.

## 4. Technical Stack Summary

| Layer | Technology | Version |
| :--- | :--- | :--- |
| **Frontend** | React / Vite | 19.2 / 7.2 |
| **Styling** | TailwindCSS | 4.1 |
| **Backend** | FastAPI | Latest |
| **Database** | PostgreSQL (Supabase) | 15+ |
| **AI Model** | Google Gemini | 2.5 Flash |
| **Language** | Python (Backend), JavaScript (Frontend) | 3.x, ESNext |

## 5. Deployment Strategy
*   **Development**: Local execution via `npm run dev` (Frontend) and `uvicorn` (Backend).
*   **Production (Recommended)**: Containerized deployment using Docker, with frontend served via CDN (e.g., Vercel/Netlify) and backend on a cloud provider (e.g., AWS/GCP/Render).
