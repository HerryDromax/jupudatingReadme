# jupudatingReadme
Its just a filtered readme file of my App "Jupudating" . The file is uploaded for HPE recruiters so that they can look at my technical writing skills. Jupudating is a live production dating app which I single handedly built over 4 months for Indian students. The app got more than 400+ downloads on Google Play Store within the 1st month of release. Checkout more at jupudating.com 

JUPU Dating
Status: Beta (v1.0.0) | Package: com.jupudating.app
A campus-exclusive dating application designed specifically for university students. Built from the ground up utilizing React Native (Expo) and Firebase, focusing on performance, secure authentication, and a seamless native feel.
Tech Stack
Layer	Technology
Framework	React Native 0.81.5 (via Expo SDK 54)
Language	TypeScript
Routing	Expo Router v6 (File-based routing)
Authentication	Firebase Auth (Email/Password & Campus Domain Verification)
Database	Firebase Firestore
Image CDN	Cloudinary
Notifications	Expo Notifications + Firebase Cloud Functions
Security	Firebase App Check (Play Integrity / App Attest)
CI/CD Build	EAS Build
Project Structure
The codebase follows a modular, feature-first architecture utilizing Expo Router for navigation:
Plaintext
app/
├── _layout.tsx               # Root layout — auth guard & navigation controller
├── index.tsx                 # Loading/splash screen
├── edit-profile.tsx          # Edit profile screen
├── modal.tsx                 # Generic reusable modal component
├── (auth)/                   # Pre-login & Onboarding Flow
│   ├── login.tsx
│   ├── human-check.tsx       # Custom Math CAPTCHA
│   └── profile-setup/        # 10-step incremental onboarding
│       ├── step1-intro.tsx   # Name, gender, intent
│       ├── step3-photos.tsx  # Photo upload (face detection + Cloudinary)
│       └── step10-bio.tsx    # Bio compilation & terms agreement
├── (tabs)/                   # Authenticated Main App (Bottom Tabs)
│   ├── index.tsx             # Discover feed
│   ├── matches.tsx           # Matches list
│   ├── chat.tsx              # Active conversations
│   └── profile.tsx           # Profile management & settings
└── chat/
    └── [id].tsx              # Dynamic individual chat room

functions/
└── index.js                  # Firebase Cloud Functions (notifications, cleanup)
Database Architecture (Firebase Collections)
The data layer is structured in Firestore to optimize for read-heavy operations like the Discover feed and real-time chat matching.
Collection	Purpose
users/{uid}	User profiles, subscription status, push tokens, and blocklists.
swipes/{id}	Records of every like/pass action (tracks sender, receiver, and action type).
matches/{uid1_uid2}	Mutual matches and metadata for the most recent message.
matches/{id}/messages	Subcollection for real-time chat messages.
reports/{id}	Moderation queue for user reports.
app_config/status	Remote killswitch for maintenance mode toggling.
Key Features
•	Strict Campus Gating: Registration is restricted to verified university email domains (e.g., @jecrcu.edu.in, @poornima.edu.in).
•	Bot Prevention & Security: Custom math CAPTCHA, mandated email verification, and Firebase App Check.
•	Smart Onboarding: 10-step incremental profile setup featuring client-side face detection for photo uploads before hitting the Cloudinary CDN.
•	Real-Time Interactions: Mutual-like matching system triggering an animated modal, backed by real-time chat with read receipts.
•	Privacy Controls: "Ghost mode" allowing users to hide from the Discover feed, alongside a robust block/report system.
•	Performance Optimization: Cloudinary image optimization via URL transformation to reduce bandwidth consumption.
Known Limitations & Technical Debt
•	Query Pagination: The 'Likes' screen is currently capped at 10 users due to Firestore in query limits. Pagination implementation is planned for v1.1.
•	Discovery Feed Optimization: The feed currently fetches all past swipes on every load. This will be migrated to a seenBy subcollection pattern to reduce document reads.
•	Indexing: The swipes collection requires a composite index on from + to + action to improve query performance at scale.
•	Offline Support: Caching and offline read capabilities are not yet fully implemented.
🧑‍💻 Developer Setup (For Teammates)
Prerequisites
•	Node.js 18+
•	Expo CLI (npm install -g expo-cli)
•	EAS CLI (npm install -g eas-cli)
First-Time Setup
1.	Clone and Install
Bash
git clone [REDACTED_REPO_URL]
cd JECRCdating
npm install
2.	Environment & Secrets
The repository includes a google-services.dev.json file pointing to our development Firebase project. This provisions automatically for dev builds.
⚠️ Security Note: Production Firebase credentials, GoogleService-Info.plist, and .env files must never be committed to version control.
Required API Keys (Request from Project Owner):
Plaintext
Cloudinary cloud name: [REDACTED_FOR_SECURITY]
Cloudinary upload preset: [REDACTED_FOR_SECURITY]
Expo project ID: [REDACTED_FOR_SECURITY]
3.	Run the Application
Bash
# Start the Metro Bundler
npx expo start

# Trigger a local development build (Android)
eas build --profile development --platform android
Deployment Rules
•	Never run firebase deploy locally. Only the project owner handles production deployments via CI.
•	Always create test accounts in the development environment. Production authentication tokens will immediately fail local validation checks.


