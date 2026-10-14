# MindSweeper — Architecture

**Course:** CPRG303 — Mobile Application Development  
**Project:** MindSweeper  
**Developer:** Ian Vanderkley  
**Project Phase:** Phase 1 — Architecture Planning  
**Target Platform:** Android  
**Status:** Proposed Architecture

## 1. Project Overview

MindSweeper is a mobile Minesweeper game designed to help players improve their problem-solving skills through gameplay, performance tracking, and AI-assisted coaching.

The application will provide two main game modes:

- **Practice Mode:** Available offline without an account. Players can practice, review their games, and track their progress.
- **Ranked Mode:** Requires an authenticated account and internet connection. Players compete for Ranked Points (RP) and progress through competitive ranks.

MindSweeper will also include an AI coaching system that provides post-game feedback, answers gameplay questions, and offers progressive hints during Practice Mode.

This is an independently developed CPRG303 project. The application will be free to use, contain no advertisements or in-app purchases, and will not be published to an application marketplace as part of the course.

### Primary User Story

> As a Minesweeper player, I want to play a complete game offline on my phone, so that I can practice and improve my skills whenever I want.

## 2. High-Level Architecture

MindSweeper will use a React Native mobile frontend, a locally executed game engine, local storage, and cloud services for online functionality.

The architecture consists of five main components:

1. **Mobile Application:** React Native and Expo will manage the user interface, navigation, settings, and gameplay interactions.
2. **Game Engine:** A separate TypeScript module will handle board generation, mine placement, revealing squares, flagging, chording, timers, and win/loss conditions.
3. **Local Storage:** SQLite will store game records and move histories. AsyncStorage will store user preferences.
4. **Cloud Backend:** Supabase will provide authentication, database storage, synchronization, and online ranked-session management.
5. **AI Coaching:** Supabase Edge Functions will communicate securely with the OpenAI API and return coaching responses to the application.

The game engine will operate independently of Supabase and the AI service. This ensures Practice Mode remains functional without internet access.

## 3. Development Framework and Dependencies

| Component | Planned Technology | Purpose |
|---|---|---|
| Mobile framework | React Native with Expo | Android application development |
| Programming language | TypeScript | Application and game logic |
| Navigation | Expo Router | Tab and stack navigation |
| UI styling | React Native StyleSheet | Component styling |
| Icons | @expo/vector-icons | Navigation and interface icons |
| Gestures | React Native Gesture Handler | Touch, pan, and pinch interactions |
| Animations | React Native Reanimated | Board movement and zoom animations |
| Local game storage | expo-sqlite | Games, boards, and move history |
| Preferences | AsyncStorage | Controls, sound, and other settings |
| Authentication | Supabase Auth | Email/password and Google Sign-In |
| Cloud database | Supabase PostgreSQL | Player data, statistics, and rankings |
| Backend | Supabase Edge Functions | Trusted operations and AI requests |
| AI integration | OpenAI API | Coaching explanations and chatbot |
| Device feedback | Expo Haptics and audio support | Optional vibration and sound effects |

Compatible package versions and any additional authentication dependencies will be confirmed during project initialization.

## 4. Navigation and User Interface

MindSweeper will use five main navigation tabs:

- **Play:** Practice and Ranked Mode selection, difficulty selection, and game access.
- **Statistics:** Lifetime performance statistics, game history, and board reviews.
- **Coaching:** AI chatbot, post-game feedback, and coaching features.
- **Leaderboard:** Public Master rankings ordered by RP.
- **Profile:** Account information, competitive rank, and access to Settings.

Expo Router will provide tab navigation for these main sections and stack navigation for gameplay, authentication, game results, board review, and settings.

The bottom navigation bar will be hidden during gameplay to maximize available screen space.

Guests will be able to access offline Practice Mode and local statistics. Ranked Mode and AI coaching will require authentication. The Master leaderboard will be publicly viewable.

### Android Controls

Players will be able to choose between two control schemes:

- **Tap & Hold:** Tap to reveal a square and hold to place or remove a flag.
- **Flag Mode Toggle:** Switch between revealing and flagging using a dedicated button.

Chording controls will also be customizable, using either a tap on an already revealed numbered square or a dedicated Chord button.

The board will support pinch-to-zoom, dedicated zoom controls, and panning. These controls are particularly important for larger Expert boards.

Settings will include separate toggles for sound effects and haptic feedback.

The application will use Android touchscreen input, network connectivity, app lifecycle detection, and optional audio and vibration. GPS, camera access, and fingerprint authentication are not required.

## 5. Game Engine and Gameplay

The game engine will be written in TypeScript and kept separate from the user interface.

### Difficulty Settings

| Difficulty | Board Size | Mines |
|---|---|---|
| Beginner | 9 × 9 | 10 |
| Intermediate | 16 × 16 | 40 |
| Expert | 30 × 16 | 99 |

All three difficulties will be available in Practice Mode. Ranked Mode will use Intermediate difficulty only.

### Gameplay Rules

- The first revealed square will always produce an empty opening. Mines will be excluded from the selected square and its surrounding neighbours.
- Players can reveal squares, place or remove flags, and use chording.
- The maximum number of placed flags equals the number of mines on the board.
- Chording reveals unflagged neighbouring squares when the adjacent flag count matches the revealed number. Incorrectly placed flags can cause a loss.
- Players win by revealing every non-mine square. Flagging every mine is not required.
- Revealing a mine ends the game as a loss.
- The timer begins with the first revealed square and stops when the game ends.

### Game Results

Completed games will display the outcome, difficulty, elapsed time, and number of moves.

Players will be able to review the final board, including mine locations and incorrect flags.

Starting a new Practice game generates a new random board. Players will be warned before abandoning an unfinished game.

## 6. Offline Storage and Synchronization

### Practice Mode

Practice Mode will run entirely on the device and will not require an account or internet connection.

Players can manually pause games. Games will also pause automatically when the application enters the background. The board will be hidden while paused.

Unfinished Practice games will be saved automatically and restored in a paused state when the application is reopened.

SQLite will store game records, board states, and individual gameplay actions. AsyncStorage will store preferences such as control methods, sound, and haptics.

### Game History

MindSweeper will retain lightweight completed-game summaries for lifetime statistics.

Detailed move histories and final board snapshots will be retained for the **100 most recent completed games**.

Lifetime statistics will include:

- Total completed games, wins, and losses
- Overall win rate
- Win rate by difficulty and game mode
- Best winning completion times
- Average winning completion times
- Historical game results

Removing older detailed histories will not affect lifetime statistics.

### Cloud Synchronization

When a player is signed in and connected, Practice results will automatically synchronize with Supabase.

Lifetime game summaries and the latest 100 detailed game records will be synchronized across devices.

Each game will have a unique identifier to prevent duplicate records. Failed uploads will be retained and retried. Unsynchronized records will not be deleted.

When a guest signs in, MindSweeper will ask permission before importing existing guest game history into the account.

Practice results will remain separate from competitive results and will never affect RP.

## 7. Authentication and Data Security

Supabase Auth will provide email/password registration and Google Sign-In.

Players will choose a unique public username containing 3–20 letters, numbers, or underscores. Usernames will be case-insensitively unique and may be changed once every 30 days.

Authentication sessions will be managed using a compatible secure mobile storage approach.

The application will support password recovery, signing out, and account deletion.

When signing out, synchronized account data will be removed from the device. Unsynchronized records must remain protected and associated with the correct account until they can be synchronized.

Permanent account deletion will require identity verification and confirmation. A trusted backend operation will remove the account and associated active records without a recovery period.

### Proposed Database Structure

| Table | Responsibility |
|---|---|
| profiles | Usernames, user IDs, account details |
| game_results | Lifetime completed-game summaries |
| game_details | Detailed board snapshots and move histories |
| ranked_sessions | Active matches, session states, recovery information |
| rankings | RP, placement progress, rank, demotion protection |
| coaching_requests | Optional coaching request records and usage information |

PostgreSQL Row Level Security will restrict access to private records.

Players will not be allowed to directly modify competitive RP. Ranked result processing will use trusted backend operations.

The public leaderboard will expose only the information needed to display eligible Master players, such as usernames and RP.

## 8. Ranked Mode Architecture

Ranked Mode will require authentication and an active internet connection.

All ranked matches will use Intermediate difficulty: 16 × 16 with 40 mines.

### Placement Matches and Scoring

New players will complete 10 placement matches before receiving their first visible rank.

- Starting RP: 0 (hidden during placements)
- Placement win: +40 RP
- Placement loss: −40 RP
- Regular ranked win: +20 RP
- Regular ranked loss: −20 RP
- Negative RP is allowed.
- Completion time is recorded but does not affect RP.

All placement matches have equal weight.

### Competitive Ranks

| Rank | Division I | Division II | Division III |
|---|---|---|---|
| Bronze | Below 100 RP | 100–199 | 200–299 |
| Silver | 300–399 | 400–499 | 500–599 |
| Gold | 600–699 | 700–799 | 800–899 |
| Platinum | 900–999 | 1,000–1,099 | 1,100–1,199 |
| Diamond | 1,200–1,299 | 1,300–1,399 | 1,400–1,499 |
| Master | 1,500+ RP | — | — |

Division I is the lowest division, and Division III is the highest. Bronze I includes negative RP values.

Master has no divisions or upper RP limit.

Only Master-ranked players appear on the global leaderboard. Falling below 1,500 RP immediately removes Master status and leaderboard eligibility.

### Demotion Protection

Demotion protection applies only at major-rank boundaries below Master.

A player can lose one match while falling below their current major rank's minimum RP without immediately losing that major rank.

A second loss while still below the threshold causes demotion.

Recovering to the rank's minimum RP resets protection. RP continues changing normally while protection is active.

Division changes within the same major rank are not protected.

Master has no demotion protection.

### Ranked Session Management

Players may have only one unfinished Ranked match at a time.

If connectivity is lost or the application enters the background, the board is hidden and the timer pauses.

Unfinished Ranked matches will support same-device recovery with a **cumulative 24-hour pause allowance**.

Each interruption consumes part of the remaining allowance. Successfully resuming stops the pause allowance from being consumed but does not reset it.

The backend will track session state and accumulated paused time using server-controlled timestamps.

When the total allowance is exhausted, the match is forfeited automatically through backend expiration processing.

Voluntary forfeits will count as losses and apply the corresponding RP deduction.

Ranked results must be processed only once, even if requests are retried.

### Competitive Integrity

The backend will manage RP changes, session status, and leaderboard eligibility.

However, client-side gameplay and basic session validation do not provide production-grade anti-cheat protection. Stronger validation of competitive gameplay will be considered for future development.

## 9. AI Coaching Architecture

MindSweeper will provide three types of AI-assisted learning:

1. General Minesweeper questions through an AI chatbot.
2. Post-game coaching based on recorded performance.
3. On-demand, progressive hints during Practice Mode.

AI features will require an authenticated account and internet connection.

Ranked Mode will not provide live board-specific coaching or hints.

### Deterministic Game Analysis

A separate TypeScript analysis module will examine Minesweeper board states using defined logical rules.

Development will begin with basic, verifiable deductions and expand to more advanced patterns as time permits.

The analysis engine will use information visible to the player when determining logically safe moves. It will not assume player-placed flags are correct or use hidden mine locations to produce live hints.

The AI will explain verified findings rather than independently determining guaranteed-safe squares.

If the analysis engine cannot prove a safe move, the application will communicate that limitation.

Post-game feedback will distinguish conclusions supported by the player's available information from observations made with hindsight.

### Progressive Practice Hints

Practice Mode will support three levels of assistance:

1. **Hint:** Direct the player's attention toward a relevant part of the board.
2. **Explanation:** Explain the logical deduction or pattern.
3. **Answer:** Reveal a specific square only when its safety has been verified.

A complete three-level hint sequence will be generated as one coaching request where AI generation is required. Existing hint levels can then be revealed without additional API calls.

Hints will be invalidated when the relevant board state changes.

### API Security and Usage Limits

The application will communicate with the OpenAI API through a Supabase Edge Function.

The function will validate authentication, request data, and usage limits. API secrets will remain on the backend.

Each account will receive **10 AI requests per UTC day**, shared across hints, chatbot messages, and post-game reviews.

The backend will enforce these limits and apply project-wide spending controls.

AI outages, request failures, or exhausted allowances will not interrupt Practice gameplay.

## 10. Testing and Reliability

MindSweeper will use automated tests for game logic and ranking calculations, along with manual Android testing for device behaviour and online integration.

| Test Area | Acceptance Criteria |
|---|---|
| Game engine | Correct mine placement, first-click protection, flagging, chording, and win/loss detection |
| Android controls | Accurate touch gestures, chording controls, zoom, and panning |
| Offline Practice | Full gameplay without internet and successful recovery of unfinished games |
| Statistics | Correct lifetime totals and win rates after detailed history pruning |
| Synchronization | Successful uploads without duplicates or lost pending records |
| Authentication | Valid login, account isolation, and protection of private data |
| Ranked Mode | Correct RP, placements, divisions, demotion protection, and leaderboard eligibility |
| Recovery | Same-device restoration within the remaining pause allowance and correct forfeiture after expiry |
| AI coaching | Verified deductions, uncertainty handling, request limits, and graceful failure |

The analysis engine will be tested using known board situations, with a longer-term target of 50–100 verified scenarios covering both straightforward and complex deductions.

Testing will be performed on an Android emulator and, where available, a physical Android device.

## 11. Development Scope and Priorities

MindSweeper will use a staged development approach, with complete gameplay prioritized over advanced online features.

### Initial MVP — Phase 4 Priorities

**Priority 1: Gameplay and Offline Practice**

- Fully playable Minesweeper with all three difficulties
- First-click protection, chording, flagging, and game results
- Customizable controls and mobile board navigation
- Offline gameplay, local statistics, and unfinished-game recovery

**Priority 2: Core Online Features**

- Email/password authentication and player profiles
- Basic Ranked Mode with placement matches and RP progression
- Competitive ranks and a basic leaderboard
- Initial cloud-backed game records

**Priority 3: Initial AI Coaching**

- Authenticated AI chatbot
- Basic post-game feedback
- Initial deterministic game analysis

### Planned Extensions

The architecture will accommodate the following features as development progresses:

- Google Sign-In
- Complete synchronization of the latest 100 detailed games
- Automated cumulative 24-hour Ranked recovery
- Three-level board-specific Practice hints
- More advanced deterministic analysis

These features may be staged beyond the initial course release if implementation or testing requires additional time.

Future enhancements may also include full move-by-move replay, stronger anti-cheat, achievements, and cooperative multiplayer.

The priority is to deliver a stable Android application with complete offline Minesweeper gameplay, functioning player progression, and an initial AI coaching experience.

## 12. Architecture Maintenance

This document records the initial architectural decisions for MindSweeper.

The architecture may evolve during implementation as technical requirements, compatibility issues, and development constraints become clearer.

Significant changes to frameworks, dependencies, storage, authentication, or application scope will be reflected in this document and tracked through the project's GitHub repository and Kanban board.
