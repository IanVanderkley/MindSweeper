# MindSweeper Architecture

**Course:** CPRG303, Mobile Application Development  
**Developer:** Ian Vanderkley  
**Project:** MindSweeper  
**Platforms:** Android and iOS  
**Stage:** Phase 1 planning

## 1. Project Overview

MindSweeper is a Minesweeper app I'm building for Android and iOS. I want the game itself to be the main focus, with a few extra features that help players practice and see how they're improving. It will have two modes. Practice Mode will work offline without an account, while Ranked Mode will require an account and an internet connection.

I'm also planning AI coaching. Players will be able to ask Minesweeper questions, get feedback after a game, and eventually request hints while practising. Live hints won't be available during Ranked games.

The app will be free, with no ads or purchases, and won't be published to an app store for this course.

**Main user story:** As a player, I want to play Minesweeper on my phone without needing internet access, so I can practise whenever I want.

## 2. How the App Will Work

I'm using React Native, Expo, and TypeScript so I can work from one codebase for Android and iOS. The game rules will run directly on the phone, separate from the screens. That way, Practice Mode won't depend on a database or API responding every time someone makes a move.

The app will have three main parts:

1. **The mobile app:** Screens, navigation, game controls, settings, and the Minesweeper engine.
2. **Local storage:** Saved games, game history, and player settings that work without internet.
3. **Online services:** Supabase for accounts, cloud data, and Ranked Mode, with an Edge Function to handle requests to the OpenAI API.

The phone will handle normal gameplay. Online services will only be needed for features such as signing in, syncing data, playing Ranked, or using AI coaching.

## 3. Tools and Dependencies

| Technology | What I'll use it for |
|---|---|
| React Native and Expo | Building the Android and iOS app |
| TypeScript | Game rules and app logic |
| Expo Router | Tabs and navigation between screens |
| React Native StyleSheet | Styling the screens |
| `@expo/vector-icons` | Icons throughout the app |
| React Native Gesture Handler | Tapping, holding, zooming, and panning |
| React Native Reanimated | Smooth board movement and zooming |
| `expo-sqlite` | Saving games, results, and move history locally |
| AsyncStorage | Control preferences and other settings |
| Supabase Auth | Player accounts and sign-in |
| Supabase PostgreSQL | Online game history, profiles, and rankings |
| Supabase Edge Functions | Secure Ranked operations and AI requests |
| OpenAI API | Chatbot and coaching explanations |
| Expo Haptics and audio support | Optional vibration and game sounds |

I'll confirm compatible package versions when I set up the Expo project. Google Sign-In is part of the longer-term plan, so it may need additional setup later.

## 4. Navigation and Mobile Controls

The app will have five main tabs:

| Tab | What players will find there |
|---|---|
| Play | Practice and Ranked Mode, difficulty selection, and starting a game |
| Statistics | Win rates, game history, best times, and board reviews |
| Coaching | AI chat and post-game feedback |
| Leaderboard | Rankings for Master players |
| Profile | Account details, current rank, and Settings |

Expo Router will handle the tabs and separate screens for gameplay, results, sign-in, and Settings. I'll hide the bottom tabs during a game to give the board more room. Guests can play Practice Mode and view local statistics, but will need to sign in for Ranked Mode or coaching. Anyone can view the Master leaderboard.

### Game controls

Players will be able to choose how they place flags. One option will be tapping to reveal and holding to flag. The other will use a button to switch between revealing and flagging. Chording will also have two options: tapping a revealed number or using a separate Chord button.

All boards will support pinch-to-zoom, zoom buttons, and panning. This matters most for Expert Mode because its 30-column board would be difficult to use if every square had to fit on the screen at once. I'll need to test the gestures carefully so dragging or zooming doesn't accidentally reveal a square.

Sound effects and haptic feedback will be optional, with separate switches in Settings. Both platforms will use touch input, network detection, and app backgrounding events. The game doesn't need GPS, camera access, or biometric authentication.

## 5. Minesweeper Gameplay

I'll keep the game engine in its own TypeScript files instead of putting all the rules inside the React Native screens. This will make it easier to test and fix gameplay without affecting the interface.

### Difficulties

| Difficulty | Board | Mines |
|---|---|---:|
| Beginner | 9 x 9 | 10 |
| Intermediate | 16 x 16 | 40 |
| Expert | 30 x 16 | 99 |

Practice Mode will offer all three difficulties. Ranked Mode will only use Intermediate.

### Game rules

- The first square a player reveals, along with the eight surrounding squares, will be safe. Mines will be placed after that first choice so it opens an empty area.
- Revealing an empty square will automatically open connected empty squares and their numbered edges.
- Players can flag hidden squares, up to the total number of mines on the board. A flagged square can't be revealed until its flag is removed.
- Chording will reveal the unflagged squares around a number when the number of surrounding flags matches it. Incorrect flags can still cause a loss.
- A game is won when every square without a mine is revealed. Players don't need to flag every mine.
- Revealing a mine ends the game. The timer starts on the first reveal and stops when the game ends.

At the end of a game, players will see whether they won or lost, their time, difficulty, and move count. They can also review the final board to see mines and incorrect flags. Starting a new Practice game will create a new random board, with a confirmation if an unfinished game would be lost.

## 6. Offline Practice, History, and Cloud Saves

Practice Mode will always work without an account or internet connection. Players can pause manually, and the game will pause automatically when the app goes into the background. The board will be hidden while paused.

I'll use SQLite to save the current board and actions as the player goes, so an unfinished Practice game can be reopened after the app closes. Recovered games will open paused. AsyncStorage will hold settings such as the chosen control scheme, sound, and haptics.

### Statistics and history

I want players to keep their lifetime results without saving every move forever. The app will keep basic summaries of completed games, including total games, wins and losses, win rates by difficulty and mode, and best and average winning times. Detailed moves and board records will be kept for the latest **100 completed games**.

Deleting older detailed records won't remove lifetime statistics. Games that haven't been synced must also be kept until they're uploaded successfully.

### Synchronization

When someone is signed in and online, completed Practice results will sync to Supabase. Each game will have its own ID, so retrying an upload won't create duplicates. The plan is to sync lifetime summaries and the latest 100 detailed records between devices, although full synchronization may be completed after the initial MVP.

If someone plays as a guest and then signs in, the app will ask before adding those guest results to their account. Practice results won't affect Ranked Points (RP). Signed-in data and unsynced guest data will also be kept separate so one account can't accidentally receive another account's history.

## 7. Accounts, Database, and Security

I'll use Supabase Auth for email and password accounts instead of building my own login system. Google Sign-In is also planned, but it isn't essential for the first working version.

Each account will have a public username with 3 to 20 letters, numbers, or underscores. Usernames will be unique regardless of capitalization, and players will only be able to change theirs once every 30 days. The app will support password resets, sign-out, and account deletion. Login sessions will use appropriate secure storage on the phone.

Supabase PostgreSQL will store online data. These are the main tables I'm planning:

| Table | Purpose |
|---|---|
| `profiles` | Player IDs and public usernames |
| `game_results` | Completed-game summaries |
| `game_details` | Board snapshots and move history |
| `rankings` | RP, placements, current rank, and demotion protection |
| `ranked_sessions` | Unfinished Ranked games and recovery state |
| `coaching_requests` | AI request usage if needed for limits and tracking |

I'll use Row Level Security so players can only access their own private records. The public leaderboard will show usernames and RP for eligible Master players, not private account details. RP changes will be handled by the backend rather than letting the app update the ranking table directly.

Signing out will clear synchronized account data stored on that device without silently deleting unsynced games. Account deletion will require confirmation and a trusted backend operation to remove the account's active records. Any provider backup retention would follow the provider's policies.

## 8. Ranked Mode

Ranked Mode will require an account and an internet connection. Every match will use a 16 x 16 board with 40 mines.

### Placement matches and RP

Players will start with 0 hidden RP and complete 10 placement games before their rank is shown. A placement win adds 40 RP and a loss removes 40 RP. After placements, wins and losses will change RP by 20. Negative RP is allowed, and game completion time won't change the points awarded.

| Rank | Division I | Division II | Division III |
|---|---|---|---|
| Bronze | Below 100 RP | 100 to 199 | 200 to 299 |
| Silver | 300 to 399 | 400 to 499 | 500 to 599 |
| Gold | 600 to 699 | 700 to 799 | 800 to 899 |
| Platinum | 900 to 999 | 1,000 to 1,099 | 1,100 to 1,199 |
| Diamond | 1,200 to 1,299 | 1,300 to 1,399 | 1,400 to 1,499 |
| Master | 1,500 RP and above | | |

Division I is the lowest division within each rank. Bronze I also includes negative RP. Master has no divisions or upper limit, and only Master players appear on the leaderboard. Dropping below 1,500 RP removes Master status immediately.

### Demotion protection

Protection will only apply when dropping out of a major rank below Master, not when moving between divisions. For example, a player at Gold I with 600 RP can lose a match and drop to 580 RP while still displaying Gold I. If they lose again without recovering, they'll be demoted. If they win enough RP to get back to 600, their protection resets. The backend will save this protection state separately from RP. Master has no protection.

### Disconnects and unfinished games

Players will only be allowed one unfinished Ranked game at a time. If the connection drops or the app goes into the background, the board will be hidden and the game timer will pause.

The longer-term recovery system will let players resume on the same device. Each match will have **24 hours of total paused time**, even if it is interrupted more than once. Resuming stops that allowance from being used but doesn't reset it. The backend will track session status and paused time using server timestamps. When the allowance runs out, the match will be forfeited and the appropriate RP deducted. Players can also choose to forfeit, with confirmation.

Match results must only be processed once, even if the app retries the same request. This is important because losing a connection should never award or deduct points twice.

### Limits of the first version

Basic Ranked Mode, RP, and the leaderboard are part of the Phase 3 plan. Fully automated recovery is a later step. Even with backend-controlled scoring, a modified app could still submit dishonest gameplay. A more secure competitive system would need stronger server-side validation, which is outside the first version's scope.

## 9. AI Coaching

The coaching features are meant to help people learn Minesweeper, not play the game for them. I'm planning a general chatbot, feedback after completed games, and optional hints during Practice Mode. All AI features will require a signed-in account and internet access. Ranked Mode won't offer live hints.

### How hints will work

I'll start with a basic TypeScript analysis system that can check Minesweeper rules and prove straightforward deductions. It will only use information a player could actually see, rather than looking at hidden mines or treating every placed flag as correct.

The OpenAI model will explain findings that the analysis system has checked. It won't be trusted to choose a guaranteed-safe square on its own. If the solver can't prove a move is safe, the app will say so instead of guessing.

The longer-term hint system will have three levels: a nudge toward the relevant area, an explanation of the logic, and the specific square if its safety is proven. The levels will come from one request where AI generation is needed, so revealing the next level won't use another request. Hints will be cleared if the board changes.

Post-game feedback can use game statistics and selected moves, but it will distinguish what the player could have known during the game from what is clear only after seeing the finished board.

### API access and limits

The app will send AI requests through a Supabase Edge Function, with the OpenAI API key kept on the server. The backend will check the account, validate the request, and enforce a limit of **10 AI requests per account per UTC day**. Chat messages, post-game reviews, and generated hint sequences will all count toward the same limit. Failed requests that never return a usable response shouldn't use up the player's allowance.

If the AI service is unavailable or the player has used their requests, Practice Mode will still work normally. The first coaching release will focus on a working chatbot and basic post-game explanations. More advanced board analysis will be added only after it can be tested properly.

## 10. Testing Plan

I'll use automated tests for the game engine and RP calculations, then test the interface and online features on Android and iOS. The Phase 3 target is at least **15 documented gameplay test cases**. I'll use an Android emulator and device where available, plus an iPhone or iOS simulator where possible.

| Area | What I'll check |
|---|---|
| Game rules | Correct mine counts, safe first reveal, flagging, chording, and win/loss results |
| Controls | Taps, holds, flag mode, chording, zoom, and panning without accidental reveals |
| Offline Practice | Playing in airplane mode and restoring a saved game after closing the app |
| Statistics | Correct lifetime totals even after older detailed records are removed |
| Cloud saves | Retrying uploads without duplicates or losing unsynced games |
| Accounts | Sign-in, privacy rules, and preventing access to other players' records |
| Ranked Mode | Placements, RP, divisions, protection, Master eligibility, and forfeits |
| AI coaching | Verified simple deductions, request limits, and handling API errors |

As the solver develops, I also want a set of known board situations with correct answers. The longer-term target is 50 to 100 examples, including situations where no guaranteed move can be proven.

## 11. Development Priorities

My main goal is to finish a reliable Minesweeper game instead of getting halfway through too many features. The development milestone is **Phase 3**. Phase 4 is the reflection, and Phase 5 is the final demonstration.

| Priority | What I'm aiming to finish |
|---|---|
| 1. Core game | Three difficulties, controls, chording, timer, results, offline Practice, saved games, and local statistics |
| 2. Basic online features | Email/password accounts, basic Ranked Mode, RP, leaderboard, and initial online game records |
| 3. Initial coaching | AI chatbot, simple post-game feedback, and basic deterministic analysis |

More complete cloud synchronization, Google Sign-In, automatic 24-hour Ranked recovery, three-level hints, and advanced solving are planned additions. Full replay, multiplayer, achievements, and stronger anti-cheat are future ideas rather than Phase 3 requirements.

If time gets tight, I'll simplify or postpone advanced online and AI features before cutting core gameplay. Any postponed features will be clearly identified in the final presentation.

## 12. Main Architecture Decisions

This section explains why I chose the main parts of the app and what I considered instead. These are development plans, not features that have already been completed.

### 12.1 One codebase for both platforms

Using React Native and Expo means I can build one game for Android and iOS instead of maintaining two separate native apps. That is a more realistic amount of work for me, and it fits what we've used in class. I'll still need to check both platforms because gestures and layouts won't always behave exactly the same.

### 12.2 Navigation with tabs and separate game screens

I chose five main tabs so players can quickly find the game, statistics, coaching, leaderboard, and profile. Putting everything in a single stack would make those sections harder to reach. The game screen will hide the tabs to make room for larger boards, but I'll need to handle paused and restored games carefully when navigating.

### 12.3 A separate TypeScript game engine

I could put the Minesweeper rules directly in the screen code, but that would make them harder to test and reuse. Keeping the engine separate lets the same rules work in Practice and Ranked Mode, with automated tests that don't depend on the interface. The extra work is keeping the board state and screen in sync.

### 12.4 Zooming and customizable controls

A fixed board that always fits the phone screen would be simpler, but Expert squares would be too small to tap reliably. Zooming, panning, and different flagging controls should make the game more usable. The downside is more work testing gestures so zooming or dragging can't accidentally reveal a mine.

### 12.5 Local saves for Practice Mode

I chose SQLite for games and AsyncStorage for settings because the main game needs to work offline. Saving everything in Supabase would be easier to manage in one place, but players couldn't practise without internet. Local storage also means I need to handle restoring unfinished games and syncing results later.

### 12.6 Keeping lifetime results without unlimited detail

I'll keep basic game summaries for lifetime statistics and detailed histories for the latest 100 games. Saving every move forever would eventually take up unnecessary space. The main thing to get right is protecting lifetime totals and games that haven't synced before deleting older detailed records.

### 12.7 Supabase for accounts and private data

I chose Supabase Auth instead of building a password system myself. It also gives me PostgreSQL and Row Level Security for protecting player records. The trade-off is learning how to manage secure mobile sessions, account permissions, and guest data properly. Google Sign-In can come after basic email and password accounts work.

### 12.8 Fixed RP and backend-controlled rankings

I chose fixed RP changes instead of ranking players by completion speed because the system will be easier to understand and test. Points will be processed through the backend rather than calculated and saved only on the phone. That protects the ranking table from direct edits, but it doesn't fully prevent cheating in a client-side game.

### 12.9 Rank protection and match recovery

Major-rank protection needs to be stored separately from RP because a player can temporarily have fewer points than their displayed rank requires. I also want interrupted matches to share one 24-hour pause allowance. Calculating rank only from RP or starting a fresh 24 hours after every disconnect would be simpler, but neither would follow the rules I've planned. Both features need extra backend work, so full recovery will come after basic Ranked Mode.

### 12.10 Checking AI hints before explaining them

I don't want the chatbot to claim a square is safe just because its answer sounds convincing. A small deterministic solver will check the move first, and the AI will explain results it can support. Asking the model to solve a board directly would be quicker, but the risk of wrong advice is too high. The first version will only cover deductions I've actually tested.

### 12.11 Keeping AI keys and usage limits on the backend

I'll use an Edge Function instead of calling OpenAI directly from the app. That keeps the API key off the phone and lets me enforce account-based request limits. It takes more backend work, but a public mobile API key would be much harder to protect and could lead to unexpected costs.

### 12.12 Building the game before expanding features

I could try to develop the full Ranked and AI systems from the start, but that would put the core game at risk. I'll get offline Minesweeper working and tested first, then add accounts, basic Ranked Mode, and initial coaching. If something runs over schedule, I'll have a complete game to demonstrate rather than several partly finished features.

## 13. Keeping This Document Updated

These are my plans at the start of development. If a technology, feature, or deadline changes during implementation, I'll update this document and record the work through GitHub Issues and my Kanban board.
