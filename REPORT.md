# Mental Health Companion: An AI-Driven Therapeutic Platform
## Academic Project Report

---

## Table of Contents
1. [Introduction to Mental Health Companion](#1-introduction-to-mental-health-companion)
2. [Literature Survey](#2-literature-survey)
3. [System Requirement Specification](#3-system-requirement-specification)
4. [System Design and Development](#4-system-design-and-development)
5. [Implementation](#5-implementation)
6. [Testing and Validation](#6-testing-and-validation)
7. [Results and Discussions](#7-results-and-discussions)
8. [Conclusion](#8-conclusion)
9. [Future Enhancements](#9-future-enhancements)
10. [Bibliography](#10-bibliography)

---

## 1. Introduction to Mental Health Companion

### 1.1 Problem Definition
In recent years, the global prevalence of mental health disorders has surged, yet accessibility to professional care remains a significant challenge. Barriers such as high costs, stigma, and a shortage of qualified professionals prevent many individuals from seeking help. There is a critical need for accessible, immediate, and private mental health support systems that can bridge the gap between users and professional therapy.

### 1.2 Objectives
The primary objective of this project is to develop "Mental Health Companion," a web-based application designed to:
*   Provide 24/7 immediate emotional support through an AI-powered conversational agent.
*   Enable users to track their emotional well-being through mood logging and journaling.
*   Facilitate professional intervention by connecting users with therapists who can monitor their progress.
*   Ensure user privacy and data security through robust authentication and access control mechanisms.

### 1.3 Scope
The project encompasses the development of a full-stack web application. The scope includes:
*   **User Interface**: A responsive, aesthetically pleasing frontend for users to interact with the AI, log moods, and access wellness resources.
*   **AI Integration**: Utilization of Large Language Models (LLMs) for natural language understanding and empathetic response generation.
*   **Therapist Portal**: A dedicated dashboard for clinicians to view client data and manage their caseload.
*   **Data Management**: Secure storage and retrieval of sensitive user data.

---

## 2. Literature Survey

### 2.1 Existing Solutions
*   **Headspace/Calm**: Focus primarily on meditation and mindfulness but lack interactive therapeutic conversation.
*   **BetterHelp/Talkspace**: Connect users with human therapists but can be prohibitively expensive and do not offer instant AI support.
*   **Woebot**: An early chatbot implementation based on decision trees, which lacks the nuance and context-awareness of modern LLMs.

### 2.2 Limitations of Current Systems
*   **Cost**: Professional therapy and premium apps are often unaffordable for students and low-income individuals.
*   **Availability**: Human therapists are not available 24/7 for immediate crisis intervention.
*   **Engagement**: Static interfaces and rule-based chatbots often fail to retain user engagement over time.

### 2.3 Proposed Solution
The Mental Health Companion addresses these limitations by combining the affordability and availability of AI with the oversight of human professionals. The use of the Gemini 2.5 Flash model allows for highly contextual and empathetic interactions that mimic human conversation more closely than previous generations of chatbots.

---

## 3. System Requirement Specification

### 3.1 Functional Requirements
1.  **User Authentication**: Users must be able to sign up, log in, and manage their profiles securely.
2.  **AI Chat Interface**: A real-time chat interface where users can send messages and receive immediate, context-aware responses.
3.  **Mood Tracking**: Users should be able to log their current mood (1-10) and attach notes.
4.  **Journaling**: A secure space for users to write detailed entries, which are analyzed for sentiment.
5.  **Therapist Dashboard**: Authorized therapists must be able to view their linked clients' mood history and journal summaries.
6.  **Crisis Detection**: The system must automatically detect keywords related to self-harm and provide immediate resources.

### 3.2 Non-Functional Requirements
1.  **Privacy**: All user data must be isolated using Row Level Security (RLS) to prevent unauthorized access.
2.  **Performance**: AI responses should be generated within 2-3 seconds to maintain conversational flow.
3.  **Scalability**: The database and backend should support concurrent users without degradation.
4.  **Usability**: The UI should be intuitive, accessible, and follow modern design principles (Glassmorphism).

### 3.3 Hardware and Software Requirements
*   **Client**: Any modern web browser (Chrome, Firefox, Edge).
*   **Server**: Python runtime environment, PostgreSQL database.
*   **Development Tools**: VS Code, Git, Node.js.

---

## 4. System Design and Development

### 4.1 System Architecture
The system follows a **Client-Server Architecture**:
*   **Frontend**: Built with React.js and Vite, providing a dynamic Single Page Application (SPA) experience.
*   **Backend**: A RESTful API built with FastAPI, handling logic and data processing.
*   **Database**: Supabase (PostgreSQL) for persistent storage.
*   **AI Service**: Google Gemini API for natural language processing.

### 4.2 Module Description
1.  **Authentication Module**: Handles JWT-based login/signup via Supabase Auth.
2.  **Chat Module**: Manages websocket/HTTP connections for messaging and interfaces with the AI service.
3.  **Wellness Module**: Handles CRUD operations for mood logs, journals, and wellness plans.
4.  **Therapist Module**: Aggregates client data for clinician review.

### 4.3 Database Design
The database schema includes the following key entities:
*   **Profiles**: Stores user identity and roles (`user` or `therapist`).
*   **Chats**: Stores message history, sender type, and emotion metadata.
*   **Mood_Logs**: Stores quantitative mood scores and labels.
*   **Therapist_Clients**: A junction table linking therapists to their clients.

### 4.4 Data Flow
Data flows from the user interface to the backend API, where it is processed (e.g., validated, sent to AI). The AI response is returned to the backend, stored in the database, and then sent back to the frontend. RLS policies intercept all database queries to ensure data security.

---

## 5. Implementation

### 5.1 Technology Stack
*   **Frontend**: React 19, TailwindCSS 4, Framer Motion.
*   **Backend**: Python 3.11, FastAPI, Pydantic.
*   **Database**: PostgreSQL 15 (via Supabase).
*   **AI**: Google Gemini 2.5 Flash.

### 5.2 Key Algorithms
**Sentiment Analysis & Crisis Detection**:
The system uses a multi-prompt approach. When a user sends a message:
1.  **Prompt A**: Generates the therapeutic response.
2.  **Prompt B**: Analyzes the text for emotion (e.g., "Anxiety", "Joy") and intensity (1-10).
3.  **Prompt C**: Scans for crisis markers. If "CRISIS" is detected, the system overrides the standard response with emergency contact information.

### 5.3 Code Implementation Details
The backend is structured using the "Router-Service-Controller" pattern to ensure separation of concerns.
*   `routers/`: Define API endpoints.
*   `services/`: Contain business logic and external API calls.
*   `core/`: Manage configuration and database connections.

---

## 6. Testing and Validation

### 6.1 Testing Strategy
*   **Unit Testing**: Individual functions (e.g., emotion analysis helper) were tested for expected outputs.
*   **Integration Testing**: API endpoints were tested using Postman to ensure correct database interaction.
*   **User Acceptance Testing (UAT)**: The UI was tested for responsiveness and ease of navigation.

### 6.2 Test Cases
| Test Case ID | Description | Expected Result | Status |
| :--- | :--- | :--- | :--- |
| TC-01 | User Login | User redirected to Dashboard on success | Pass |
| TC-02 | AI Response | Chatbot replies within 3 seconds | Pass |
| TC-03 | Crisis Alert | Inputting "I want to hurt myself" triggers alert | Pass |
| TC-04 | Data Privacy | User A cannot see User B's journal | Pass |

---

## 7. Results and Discussions

### 7.1 User Interface
The application features a modern, "Glassmorphism" design with a dark theme to reduce eye strain and create a calming atmosphere. The dashboard provides clear visualizations of mood trends using charts.

### 7.2 Performance Analysis
*   **Latency**: Average API response time is <200ms (excluding AI generation).
*   **AI Latency**: Gemini responses average 1.5-2.5 seconds, which is acceptable for a conversational interface.

### 7.3 Impact
The system successfully provides a private, judgment-free space for users to express themselves. The mood tracking features allow users to gain insights into their emotional patterns over time.

---

## 8. Conclusion
The "Mental Health Companion" project successfully demonstrates the potential of integrating advanced AI with standard web technologies to create a supportive mental health tool. By addressing the key limitations of cost and availability, the platform serves as a valuable supplement to professional therapy. The robust security measures ensure that user trust is maintained, which is paramount in digital health applications.

---

## 9. Future Enhancements
1.  **Mobile Application**: Developing a React Native version for better accessibility on smartphones.
2.  **Voice Interaction**: Integrating Speech-to-Text and Text-to-Speech for a hands-free therapy experience.
3.  **Wearable Integration**: Syncing with smartwatches (e.g., Apple Watch, Fitbit) to correlate physiological data (heart rate, sleep) with mood logs.
4.  **Multi-language Support**: Expanding the AI's capabilities to support non-English speakers.

---

## 10. Bibliography
1.  **FastAPI Documentation**: https://fastapi.tiangolo.com/
2.  **React Documentation**: https://react.dev/
3.  **Google Gemini API**: https://ai.google.dev/
4.  **Supabase Documentation**: https://supabase.com/docs
5.  Calvo, R. A., & D'Mello, S. (2010). *Affect Detection: An Interdisciplinary Review of Models, Methods, and Their Applications*. IEEE Transactions on Affective Computing.
