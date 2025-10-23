# TriggerWeave MVP Product Requirements Document (PRD)

## 1. Product Overview
### 1.1 Product Name
TriggerWeave

### 1.2 Product Description
TriggerWeave is a mobile iOS app designed to help busy professionals and remote workers identify and manage micro-vices (subtle unhealthy or unproductive habits like excessive caffeine intake, stress snacking, or minor doom scrolling) through a simple, chat-like interface. Users interact primarily via voice notes (e.g., "I just smoked a cigarette" or "I had two cups of coffee this morning"), which the app transcribes and analyzes, or via direct text input for logging. The app passively establishes a baseline of habits over the first 7-14 days by correlating logs with device data (e.g., time, location, calendar events). Once the baseline is set, it unlocks AI-driven insights, such as trigger forecasts and personalized micro-adjustments, to promote sustainable changes without rigid tracking or overwhelming data.

This MVP focuses on core functionality for voice-based and text-based logging, baseline analysis, and basic insights, with a freemium model (basic features free; advanced AI insights behind a paywall).

### 1.3 Version
MVP v1.0

### 1.4 Date
October 23, 2025

### 1.5 Stakeholders
- Product Owner: [User/Founder]
- Development: AI-Coding Agent (e.g., based on tools like Cursor, GitHub Copilot, or custom agents)
- Target Users: Busy professionals aged 25-45 dealing with micro-vices in hybrid/remote work environments

## 2. Goals and Objectives
### 2.1 Business Goals
- Validate the concept of voice-note-based and text-based vice tracking with at least 1,000 beta users via TestFlight.
- Achieve 30% retention rate after 30 days by providing subtle, non-intrusive insights.
- Monetize through in-app purchases for premium features, targeting $5K in revenue within 6 months post-launch.
- Differentiate from existing habit trackers by focusing on preventive micro-adjustments rather than reactive logging.

### 2.2 User Goals
- Easily log micro-vices hands-free via voice or via quick text input for flexibility.
- Gain self-awareness through automated baseline establishment and pattern insights.
- Receive practical, low-effort suggestions to weave in better habits.

### 2.3 Success Metrics
- User Acquisition: 500+ downloads in first month.
- Engagement: Average 3+ logs (voice or text) per user per week.
- Conversion: 10% of free users upgrade to premium.
- Feedback: NPS score >7 from beta testers.

## 3. Target Audience and Personas
### 3.1 Primary Users
- Busy professionals (e.g., remote workers, executives) who experience micro-vices tied to daily routines but lack time for detailed tracking apps.
- Users who prefer voice for hands-free logging or text for precision and quiet environments.

### 3.2 User Personas
- **Alex the Analyst**: 32-year-old data analyst working remotely; struggles with afternoon caffeine binges during meetings; wants quick voice or text logs and alerts without app overload.
- **Sam the Sales Rep**: 28-year-old field sales worker; deals with stress snacking during commutes; needs offline-capable, hands-free voice or text tracking with calendar integration.

### 3.3 Market Context
- Competes indirectly with general habit trackers (e.g., Loop Habit Tracker) but differentiates via voice-first with text support, AI-preventive focus.
- Addresses gap in micro-vice management amid rising wellness awareness (e.g., 70% of professionals report habit struggles per wellness surveys).

## 4. Features and Requirements
### 4.1 MVP Scope
Focus on iOS-only (iPhone, iOS 17+). Prioritize core loop: Log (voice or text) → Analyze → Insight. Exclude advanced features like social sharing or Android support for MVP.

### 4.2 Core Features
1. **Onboarding**
   - Quick setup flow: Welcome screen, select 3-5 predefined micro-vices (e.g., caffeine, snacking, scrolling) or add custom.
   - Request permissions: Microphone (for voice), notifications, calendar (optional for correlations).
   - Explain baseline phase (7 days minimum) and logging options (voice notes or text input).
   - Requirements: Single-screen carousel; store selections in UserDefaults.

2. **Chat-Like Interface for Logging**
   - Main screen: Scrollable chat thread (user bubbles on right, app responses on left).
   - Voice Recording: Tap mic button to record (up to 30 seconds); auto-transcribe and add to thread.
   - Text Logging: Tap text input field to type directly (e.g., "I had two cups of coffee this morning"); add to thread with auto-categorization.
   - Example Interaction: User types or says "I had two cups of coffee this morning" → App categorizes (caffeine), replies "Logged at 9 AM. Noted as caffeine intake."
   - Offline support: Logs stored locally.
   - Requirements: Use AVFoundation for recording, Speech framework for transcription (English primary; handle accents gracefully). Standard UITextField for text input.

3. **Baseline Establishment**
   - Passive analysis over 7-14 days: Parse logs (transcripts or text) for keywords (e.g., regex for "coffee" → caffeine).
   - Correlate with device data: Timestamp, location (optional), calendar events (via EventKit).
   - Store data locally in Core Data: Entries include {id, timestamp, content (transcript or text), vice_category, context_data}.
   - After baseline (e.g., 10+ logs): Unlock insights tab.
   - Requirements: Simple on-device processing; no cloud for MVP baseline.

4. **AI-Driven Insights and Nudges**
   - Insights Tab: Visual reports (e.g., frequency charts via Charts framework) like "Caffeine logs peak at 3 PM."
   - Trigger Forecasts: Predictive alerts (e.g., notification: "3 PM slump approaching—try a walk instead of coffee?").
   - Micro-Adjustments: Chat suggestions like "Swap coffee for herbal tea during meetings?"
   - Premium Unlock: Advanced AI (e.g., using OpenAI API for natural language insights) via subscription.
   - Requirements: Basic ML with Core ML for patterns; integrate OpenAI SDK for premium (prompt example: "Analyze these logs for triggers and suggest swaps").

5. **Settings and Premium**
   - Export data (CSV via share sheet).
   - Manage vices, notifications, and privacy.
   - In-App Purchase: $4.99/month for premium AI (use StoreKit).
   - Requirements: Basic settings screen; handle subscription states.

### 4.3 Non-Functional Requirements
- **Performance**: Load times <2 seconds; handle 100+ logs without lag.
- **Security/Privacy**: On-device data storage; encrypt sensitive logs with Keychain. Comply with GDPR/Apple privacy.
- **Accessibility**: VoiceOver support; high-contrast UI.
- **UI/UX Guidelines**: Clean, minimalist theme (soft blues/greens). Use SwiftUI for responsive design.
- **Error Handling**: Graceful fallbacks (e.g., text input if mic fails or is denied).

## 5. User Flows
### 5.1 Key Flows
1. **First Launch/Onboarding**:
   - Open app → Welcome → Select vices → Permissions → Home chat screen with prompt: "Record a voice note or type your first log!"

2. **Logging a Vice**:
   - Tap mic for voice or text field for typing → Input (e.g., "Smoked a cigarette") → Categorize → App reply → Add to thread.

3. **Baseline to Insights**:
   - After 7 days/10 logs → Notification: "Baseline ready!" → Insights tab unlocks → View reports → Receive first forecast.

4. **Premium Upgrade**:
   - Tap premium insight → Paywall prompt → Purchase → Unlock advanced features.

### 5.2 Edge Cases
- No internet: Full offline logging; sync insights when online (if using API).
- Poor transcription: Allow edit button on voice logs to correct via text.
- Permission denied (mic): Default to text input mode.
- Mixed logging: Seamlessly handle both voice and text in the same thread.

## 6. Technical Specifications
### 6.1 Tech Stack
- **Language/Framework**: Swift with SwiftUI (primary UI); UIKit for any complex elements.
- **iOS Frameworks**:
  - Speech: Transcription (for voice).
  - AVFoundation: Audio recording (for voice).
  - EventKit: Calendar integration.
  - Core ML: Basic pattern analysis.
  - UserNotifications: Alerts.
  - Charts: Visual reports.
  - StoreKit: In-app purchases.
  - Core Data: Local storage.
- **Backend/Integrations**:
  - Firebase: User auth, analytics (free tier); optional cloud sync.
  - OpenAI API: Premium insights (SDK via Swift Package Manager; budget for API keys).
- **Tools**:
  - Xcode: Development.
  - GitHub: Version control.
  - TestFlight: Beta distribution.
- **Dependencies**: Minimize; use Swift Package Manager for any externals (e.g., OpenAI).

### 6.2 Development Guidelines for AI-Coding Agent
- Build in phases: Start with onboarding/UI, then logging (voice and text), analysis, insights, premium.
- Use modular code: Separate concerns (e.g., ViewModels for logic).
- Testing: Include unit tests (XCTest) for transcription, text parsing, and purchases.
- Documentation: Comment code; generate README with setup instructions.
- Assumptions: Agent has access to Xcode/Mac environment; handle API keys as environment variables.

## 7. Assumptions and Dependencies
- **Assumptions**: Users have iOS 17+ devices; English as primary language for MVP.
- **Dependencies**: Apple Developer Account ($99/year); OpenAI API key for premium.
- **Risks**: Transcription accuracy (mitigate with text edits and fallback); App Store review delays (plan 2 weeks buffer).
- **Out of Scope for MVP**: Multi-language, Android, advanced ML training, community features.

## 8. Timeline and Milestones
- **Week 1**: Setup project, onboarding, and basic UI.
- **Weeks 2-3**: Logging (voice and text) and baseline analysis.
- **Weeks 4-5**: Insights, notifications, and premium integration.
- **Week 6**: Testing, bug fixes, App Store prep.
- **Launch**: Submit to App Store; monitor via Firebase.

This PRD provides a clear blueprint for the AI-coding agent to build the MVP. If refinements are needed (e.g., wireframes or code starters), provide feedback!
