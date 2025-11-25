# Mental Health Companion App - Comprehensive Documentation Report

## 1. Introduction
The Mental Health Companion App is a holistic, AI-powered platform designed to support users in their mental wellness journey. It combines evidence-based therapeutic techniques with modern gamification and AI-driven personalization to create an engaging and effective mental health tool. The platform caters to both individual users seeking self-care tools and therapists managing client progress.

## 2. System Architecture
The application follows a modern client-server architecture:

### Frontend
- **Framework**: React (Vite)
- **Styling**: Tailwind CSS, Glassmorphism design system
- **Visuals**: Spline (3D), Lucide React (Icons), Framer Motion (Animations)
- **State Management**: React Hooks, Context API (implied)
- **Routing**: React Router DOM

### Backend
- **Framework**: FastAPI (Python)
- **Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **AI Engine**: Google Gemini (Generative AI)
- **Real-time**: WebSockets (for Symphony feature)

### Infrastructure
- **Hosting**: Localhost (currently), deployable to Vercel/Render/Railway
- **External Services**: Google Generative AI API, Supabase Platform

## 3. Core Features

### 3.1 Authentication
- **Purpose**: Secure user access and role management.
- **Tech Stack**: Supabase Auth, React Context.
- **Flow**: Users sign up/login via email/password. Supabase handles session management.
- **Roles**: 'user' (default) and 'therapist'.
- **Files**: `backend/app/routers/auth.py`, `frontend/src/pages/Login.jsx`

### 3.2 Dashboard
- **Purpose**: Central hub for wellness overview and daily planning.
- **Features**:
    - **Personalized Plan**: AI-generated daily plan (Quote, Tasks, Focus Tip) based on mood and focus stats.
    - **Statistics**: Visual breakdown of mood logs, focus minutes, journal entries, and streaks.
    - **Quick Actions**: Fast access to core features.
- **Internal Working**: Fetches aggregated stats from Supabase. Calls `wellness/plan` endpoint which uses Gemini to generate a plan.
- **Files**: `backend/app/routers/wellness.py`, `frontend/src/pages/Dashboard.jsx`

### 3.3 Focus Mode
- **Purpose**: Enhance productivity using the Pomodoro technique.
- **Features**:
    - **Timer**: Customizable duration (default 25m).
    - **Visualizations**: 'Galaxy' (star field) and 'Calm Lake' (ambient) themes.
    - **Gamification**: Completing sessions awards XP to the 'Oak' plant in Mind Garden.
- **Internal Working**: Frontend timer logic. On completion, sends session data to backend and triggers XP award.
- **Files**: `backend/app/routers/focus.py`, `frontend/src/pages/Focus.jsx`

### 3.4 Meditation
- **Purpose**: Guided relaxation and mindfulness.
- **Features**:
    - **Themes**: Forest Rain, Mountain Breeze, Zen Garden (Visual + Audio).
    - **Timer**: Adjustable duration.
    - **Gamification**: Completing sessions awards XP to the 'Lotus' plant.
- **Internal Working**: Frontend timer and audio playback. Backend logs activity and awards XP.
- **Files**: `frontend/src/pages/Meditation.jsx`, `backend/app/routers/garden.py`

### 3.5 Journaling & Art Therapy
- **Purpose**: Emotional expression and processing.
- **Features**:
    - **Text Journal**: Rich text entry with tagging.
    - **Sentiment Analysis**: AI analyzes content to detect mood (Positive/Negative/Neutral) and intensity.
    - **Emotion-to-Art**: AI generates visual art based on the user's emotional state (Art Therapy).
    - **Gamification**: Entries award XP to the 'Sunflower'.
- **Internal Working**: Text sent to `journal/` endpoint. Gemini analyzes sentiment. For art, Gemini generates a prompt, and an image generation model (simulated or integrated) creates the image.
- **Files**: `backend/app/routers/journal.py`, `frontend/src/pages/Journal.jsx`

### 3.6 AI Therapy Chat
- **Purpose**: On-demand conversational support.
- **Modes**:
    - **Gentle Listener**: Empathetic, non-judgmental support.
    - **Conversational Coach**: Action-oriented guidance.
    - **Silent Space**: Minimalist presence.
    - **Rehearsal Room**: Roleplay difficult conversations.
- **Internal Working**: Frontend sends message history to `therapy/chat`. Backend constructs a prompt for Gemini based on the selected persona/mode and returns the response.
- **Files**: `backend/app/routers/therapy.py`, `frontend/src/pages/Therapy.jsx`

### 3.7 Mind Garden
- **Purpose**: Visual metaphor for mental growth (Gamification).
- **Features**:
    - **Plants**: Lotus (Meditation), Oak (Focus), Sunflower (Journaling), Rose (Games), Star (Helping others).
    - **Growth Stages**: Plants grow visually as users earn XP in respective categories.
    - **Seasons**: Garden changes appearance based on user activity frequency (Spring, Autumn, Winter).
- **Internal Working**: `mind_garden` table tracks XP per plant type. Backend logic determines level and season. Frontend renders SVG plants based on stage.
- **Files**: `backend/app/routers/garden.py`, `frontend/src/pages/MindGarden.jsx`

### 3.8 Symphony (The Lighthouse)
- **Purpose**: Anonymous collective emotional connection.
- **Features**:
    - **Star Map**: Users appear as glowing stars on a shared canvas.
    - **Mood Colors**: Star color reflects current mood.
    - **Pulse**: Users can send "pulses" (positive messages) to others anonymously.
- **Internal Working**: WebSocket connection (`symphony/ws`). Real-time broadcasting of user positions and states.
- **Files**: `backend/app/routers/symphony.py`, `frontend/src/pages/Symphony.jsx`

### 3.9 Mind Games
- **Purpose**: Cognitive training and stress relief.
- **Games**: Breathing Bubble, Memory Match, Mood Flip, Focus Flow.
- **Gamification**: High scores award XP to the 'Rose'.
- **Files**: `frontend/src/pages/Games.jsx`

### 3.10 Content Library
- **Purpose**: Educational resources.
- **Features**: Curated list of articles, videos, podcasts, and tools. Filterable by type and tags.
- **Files**: `backend/app/routers/content.py`, `frontend/src/pages/Content.jsx`

### 3.11 Therapist Dashboard
- **Purpose**: Client management for professional users.
- **Features**:
    - **Client List**: View linked clients.
    - **Client Data**: Access client mood logs and journal summaries (with permission).
    - **Invite System**: Link new clients via email.
- **Internal Working**: Queries `therapist_clients` table. Aggregates data from `mood_logs` and `journal_entries`.
- **Files**: `backend/app/routers/therapist.py`, `frontend/src/pages/TherapistDashboard.jsx`

## 4. Technical Details

### Database Schema (Key Tables)
- `profiles`: User details, roles.
- `journal_entries`: Content, mood analysis, art prompts.
- `mood_logs`: Daily mood scores.
- `focus_sessions`: Duration, task name.
- `mind_garden`: Plant types, XP, levels.
- `therapist_clients`: Relationships between therapists and users.

### AI Integration
- **Model**: Google Gemini (via `google-generativeai` SDK).
- **Use Cases**:
    - Wellness Plan Generation
    - Sentiment Analysis
    - Therapy Chat Personas
    - Rehearsal Roleplay
    - Art Prompt Generation

## 5. Research Insights & Novelty
- **Adaptive AI Personas**: The therapy chat adapts its tone and strategy based on the user's selected mode, providing tailored support ranging from passive listening to active coaching.
- **Visual Biofeedback**: The Mind Garden creates a direct feedback loop between positive actions (meditation, focus) and visual rewards (plant growth), reinforcing healthy habits through operant conditioning.
- **Anonymous Social Connection**: Symphony provides a sense of "being alone together," reducing isolation without the pressure of direct social interaction, utilizing the concept of "parallel play" in a digital space.
