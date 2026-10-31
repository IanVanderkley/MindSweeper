# MindSweeper — Phase 2 Interface Scope

## Overview

This document outlines how each MindSweeper screen will work, based on the nine user flows in [UserFlows.md](UserFlows.md). It covers what players see, what they can do, how they move between screens, and the feedback they receive.

The app has five main tabs: Play, Statistics, Coaching, Leaderboard, and Profile. Other screens open through stack navigation or confirmation dialogs. Practice works offline without an account. Ranked and AI Coaching require sign-in and internet access, while guests can still view the public Master leaderboard.

Each screen covers its purpose, information, player actions, controls, navigation, and important states or feedback.

## 1. Play, Practice, and Ranked

### Play (`play.svg`)

**Supports user flow(s):** 1, 2, 3

- **Purpose:** Give players a starting point for Practice or Ranked.
- **Information shown:** Practice selected by default, Beginner/Intermediate/Expert difficulties (9×9/10 mines, 16×16/40 mines, 16×30/99 mines), and the two game modes.
- **Player actions/input:** Choose a mode and difficulty; start Practice or enter Ranked.
- **Primary controls:** Mode selector, difficulty choices, Start Practice and Ranked entry controls, five-tab navigation.
- **Navigation:** Practice opens full-screen gameplay. Ranked checks sign-in and internet access before proceeding.
- **States and feedback:** If an unfinished Practice game exists, the Play area offers Resume Game and protects it from accidental replacement.

### Practice game (`practice-game.svg`)

**Supports user flow(s):** 1, 2

- **Purpose:** Let the player complete a Minesweeper game without an account or internet.
- **Information shown:** Board, timer, mine/flag counter, and active gameplay state.
- **Player actions/input:** Reveal a square, place or remove a flag, chord when allowed, zoom, pan, or pause.
- **Primary controls:** Tappable board, long-press flagging or Flag Mode, optional Chord button or number-tap chording, zoom buttons and pinch/pan gestures, Pause control.
- **Navigation:** Opens from Play or Resume; goes to paused state when paused and to Victory or Game Over when finished.
- **States and feedback:** First revealed square opens a safe area. Valid moves update the board and counters. A win or mine reveal ends gameplay; starting over must not silently discard an unfinished game.

### Paused Practice (`practice-paused.svg`)

**Supports user flow(s):** 2

- **Purpose:** Hold an unfinished Practice game safely until the player resumes.
- **Information shown:** Saved game board remains visible, with the timer stopped.
- **Player actions/input:** Resume the current game or leave it saved for later.
- **Primary controls:** Resume Game button; the board remains visible but disabled.
- **Navigation:** Resume returns to active Practice. Leaving goes to Play with Resume Game available.
- **States and feedback:** Players cannot reveal or flag squares while the game is paused. If they switch to another app or close MindSweeper, the game will automatically save and pause.

### Resume Game (`resume-game.svg`)

**Supports user flow(s):** 2

- **Purpose:** Show the player that an unfinished Practice game can be continued.
- **Information shown:** Resume Game option on Play when a saved session exists.
- **Player actions/input:** Resume the saved game or choose to start a different one.
- **Primary controls:** Resume Game and Start Practice controls.
- **Navigation:** Resume opens the saved game in a paused state; starting a new game opens Replace Saved Game confirmation.
- **States and feedback:** The saved game stays intact until the player chooses to replace it. Resume Game is another state of the Play screen.

### Replace Saved Game (`replace-saved-game.svg`)

**Supports user flow(s):** 2

- **Purpose:** Prevent accidental loss of unfinished Practice progress.
- **Information shown:** Warning that beginning another Practice game replaces the saved session.
- **Player actions/input:** Choose Cancel or Replace.
- **Primary controls:** Cancel and Discard & Start New buttons.
- **Navigation:** Cancel returns to the existing saved-game choice; Replace starts a fresh Practice game.
- **States and feedback:** Cancel changes nothing. Replace deliberately removes the earlier unfinished session.

### Victory (`victory.svg`)

**Supports user flow(s):** 1

- **Purpose:** Show a completed Practice win and the player's next choices.
- **Information shown:** Victory result, difficulty, elapsed time, move count, and flags.
- **Player actions/input:** Play again, review final board, return to Play, or request AI analysis.
- **Primary controls:** Four result actions; final board is available through Game Details.
- **Navigation:** Play Again opens a new Practice game; Review opens Game Details; Return goes to Play; AI analysis follows Coaching access requirements.
- **States and feedback:** Gameplay is completed and the result no longer accepts moves. AI analysis does not start automatically.

### Game Over (`game-over.svg`)

**Supports user flow(s):** 1

- **Purpose:** Explain that the Practice game ended in a loss.
- **Information shown:** Loss result, difficulty, time, moves, and flags, with an option to review the final board.
- **Player actions/input:** Play again, review final board, return to Play, or request AI analysis.
- **Primary controls:** Result actions corresponding to Victory.
- **Navigation:** Uses the same destinations as Victory.
- **States and feedback:** The game is over after a mine is revealed; mistakes can be reviewed without making further moves.

### Account Required (`account-required.svg`)

**Supports user flow(s):** 3, 7

- **Purpose:** Explain why an unauthenticated player cannot enter Ranked.
- **Information shown:** Message that Ranked requires an account.
- **Player actions/input:** Sign in, create an account, or cancel.
- **Primary controls:** Sign In, Create Account, and Cancel controls.
- **Navigation:** Account options open the account flow; Cancel returns to Play. After sign-in and any guest-history choice, the player returns to the feature they selected.
- **States and feedback:** Ranked does not start for guests; Practice remains available.

### Ranked Offline (`ranked-offline.svg`)

**Supports user flow(s):** 3

- **Purpose:** Explain why Ranked cannot start without an internet connection.
- **Information shown:** Connection requirement and availability of offline Practice.
- **Player actions/input:** Retry the connection or return to Play.
- **Primary controls:** Retry Connection and Back to Play buttons.
- **Navigation:** A successful retry can continue the Ranked entry flow. Back to Play returns to the Play tab.
- **States and feedback:** Ranked cannot start without a connection. The message explains why.

### Ranked Placements (`ranked-placements.svg`)

**Supports user flow(s):** 3

- **Purpose:** Guide a new Ranked player through the 10 placement matches.
- **Information shown:** Placement count and progress (for example 5/10), fixed Intermediate difficulty, and match-start action.
- **Player actions/input:** Start the next placement match.
- **Primary controls:** Placement progress indicator, Start Placement Match, and Ranked Rules controls.
- **Navigation:** Opens Ranked gameplay, then returns through Ranked Results to the next placement or, after ten games, the Ranked Lobby.
- **States and feedback:** Rank and RP remain hidden during placements; progress advances only after a result is checked.

### Ranked Lobby (`ranked-lobby.svg`)

**Supports user flow(s):** 3

- **Purpose:** Let a placed player review their rank and start a Ranked match.
- **Information shown:** Rank badge, rank name, RP, progress to the next division, personal global position, Ranked win rate, and Intermediate difficulty.
- **Player actions/input:** Start a Ranked match, view the Master leaderboard, or return to Play.
- **Primary controls:** Start Ranked Match, View Master Leaderboard, rank/progress display, and Back.
- **Navigation:** Start Ranked Match opens full-screen gameplay, which leads to Ranked Results. The leaderboard option opens the Leaderboard tab.
- **States and feedback:** The lobby is available after sign-in and an internet check. Rank and RP show the latest verified results.

### Ranked Results (`ranked-results.svg`)

**Supports user flow(s):** 3

- **Purpose:** Tell the player the outcome of a verified Ranked match.
- **Information shown:** Win or loss, any RP change, and updated Ranked position or placement progress when applicable.
- **Player actions/input:** Return to Ranked and choose whether to play another match.
- **Primary controls:** Return to Ranked Lobby button and result summary.
- **Navigation:** Returns to the next placement or Ranked Lobby, where another match can be started.
- **States and feedback:** RP changes appear as final only after the match is verified. The same match cannot change RP twice.

## 2. Statistics and Game History

### Practice Statistics (`practice-statistics.svg`)

**Supports user flow(s):** 4

- **Purpose:** Show progress in offline Practice games, including for guests.
- **Information shown:** Games played, wins, losses, win rate, best times, and the selected difficulty filter.
- **Player actions/input:** Switch between All, Beginner, Intermediate, and Expert; open Game History.
- **Primary controls:** Difficulty filter, statistics cards, Game History entry, Statistics tab navigation.
- **Navigation:** History opens in a stack and Back returns to Statistics.
- **States and feedback:** Figures change with the filter; an account is not required to see local Practice statistics.

### Ranked Statistics (`ranked-statistics.svg`)

**Supports user flow(s):** 4

- **Purpose:** Show the player's competitive results.
- **Information shown:** Played/won/lost counts, win rate, current rank and RP, global position, Best Ranked Time, and All-Time Highest Ranked.
- **Player actions/input:** Review statistics and open Game History.
- **Primary controls:** Rank displays, separate best-time/highest-rank cards, history entry.
- **Navigation:** Opened through Statistics mode selection; history and details use stack navigation.
- **States and feedback:** Rank and RP reflect checked Ranked results. Signed-out players cannot view account-specific Ranked statistics.

### Game History (`game-history.svg`)

**Supports user flow(s):** 4, 5

- **Purpose:** Allow the player to find a previous completed game.
- **Information shown:** Recent completed games with their result, difficulty, completion time, and date. The same layout will support Practice and Ranked history.
- **Player actions/input:** Choose a completed game or return to Statistics or the originating flow.
- **Primary controls:** Selectable history entries and Back control.
- **Navigation:** Selecting a game opens Game Details; Coaching may also use a selected completed game for analysis.
- **States and feedback:** An empty-history state explains when there are no completed games to review.

### Game Details (`game-details.svg`)

**Supports user flow(s):** 1, 4, 5

- **Purpose:** Let the player inspect the final board without changing the game.
- **Information shown:** Revealed numbers, mines, placed flags, incorrect flags, and final game information.
- **Player actions/input:** Zoom, pan, centre the board, or go back.
- **Primary controls:** Read-only board, zoom/centre controls, Back.
- **Navigation:** Returns to history, a result screen, or a Coaching report depending on where it was opened.
- **States and feedback:** Board input is disabled. Incorrect flags are visibly identified.

## 3. AI Coaching

### Coaching (`coaching.svg`)

**Supports user flow(s):** 1, 5

- **Purpose:** Give the player access to questions and completed-game analysis.
- **Information shown:** Ask a Question and Analyze a Completed Game options, plus remaining daily requests.
- **Player actions/input:** Choose a coaching task or follow the sign-in requirement.
- **Primary controls:** Two entry controls, request counter, Coaching tab navigation.
- **Navigation:** Chat and Report open in a stack; a Practice result can lead here for analysis.
- **States and feedback:** Signed-out, offline, and exhausted-request cases show why Coaching is unavailable. Daily allowance resets at 00:00 UTC.

### Coaching Chat (`coaching-chat.svg`)

**Supports user flow(s):** 5

- **Purpose:** Allow the player to ask a Minesweeper question and read the answer.
- **Information shown:** Conversation, submitted question, returned explanation, and remaining requests.
- **Player actions/input:** Type and submit a question; return to Coaching.
- **Primary controls:** Text input, Send control, chat messages, Back.
- **Navigation:** Opens from Coaching and returns there with Back.
- **States and feedback:** Sending a question shows a loading state before the answer appears. If the service fails, the player sees an error. A failed request without a usable answer does not reduce the daily allowance.

### Coaching Report (`coaching-report.svg`)

**Supports user flow(s):** 1, 5

- **Purpose:** Explain what a player did well and could improve in a completed game.
- **Information shown:** Performance grade, positive actions, areas to improve, and explanations connected to the finished board.
- **Player actions/input:** Read the report, review Game Details, or return to Coaching.
- **Primary controls:** Report sections, Review Final Board control, Back.
- **Navigation:** Opens from a selected completed game or Practice result; Game Details opens in a stack.
- **States and feedback:** The report covers the game the player selected. Completing a game does not automatically use an AI request.

### AI Request Limit (`ai-request-limit.svg`)

**Supports user flow(s):** 5

- **Purpose:** Explain that the player's daily coaching allowance has been exhausted.
- **Information shown:** No requests remain and the next reset occurs at 00:00 UTC.
- **Player actions/input:** Return to Coaching.
- **Primary controls:** Limit message and Got It button.
- **Navigation:** Returns to the Coaching home screen.
- **States and feedback:** Players cannot submit another AI request until the allowance resets.

## 4. Leaderboard

### Guest Leaderboard (`guest-leaderboard.svg`)

**Supports user flow(s):** 6

- **Purpose:** Let guests view the Master ranking list without an account.
- **Information shown:** Public list of current Master players, including position, username, RP, and Master emblem.
- **Player actions/input:** View the list or choose to sign in.
- **Primary controls:** Leaderboard list, Sign In entry, tab navigation.
- **Navigation:** Sign In opens in a stack; the player can return to the public leaderboard.
- **States and feedback:** Personal ranking area displays 'Sign in to see your global position.' No public player-profile navigation is required.

### Leaderboard (`leaderboard.svg`)

**Supports user flow(s):** 6

- **Purpose:** Show the public Master list and a signed-in player's personal rank information.
- **Information shown:** Current Master player positions/usernames/RP; own global position after placements, or placement progress beforehand.
- **Player actions/input:** View the standings and personal position.
- **Primary controls:** Ranked list, personal ranking/progress area, tab navigation.
- **Navigation:** Remains within the Leaderboard tab; no other-player profile screen opens from list entries.
- **States and feedback:** Only Master players appear in the public list. Lower-ranked players can still see their own separate global position after placements.

## 5. Profiles and Accounts

### Guest Profile (`guest-profile.svg`)

**Supports user flow(s):** 7, 8

- **Purpose:** Give guests access to account creation, sign-in, and permitted settings.
- **Information shown:** Guest account state and account access options.
- **Player actions/input:** Sign in, create an account, or open Settings.
- **Primary controls:** Sign In/Create Account entries and Settings access.
- **Navigation:** Account screens and Settings open in a stack; the bottom Profile tab remains the entry point.
- **States and feedback:** Practice remains available without sign-in; account-only features remain gated.

### Profile (`profile.svg`)

**Supports user flow(s):** 7, 8, 9

- **Purpose:** Show the signed-in player's account area and access to Settings.
- **Information shown:** Player identity, account information, current competitive rank, and Settings entry.
- **Player actions/input:** Open Settings or sign out.
- **Primary controls:** Profile content, Settings, Sign Out, and tab navigation.
- **Navigation:** Settings opens in a stack from Profile. Signing out returns to Guest Profile.
- **States and feedback:** This state is shown after sign-in and any guest-history choice; signing out returns to Guest Profile.

### Sign In (`sign-in.svg`)

**Supports user flow(s):** 3, 5, 6, 7

- **Purpose:** Authenticate a player for online features.
- **Information shown:** Email and password fields, Forgot Password, and 'No account needed for practice.'
- **Player actions/input:** Enter credentials, show/hide password, sign in, reset password, or return.
- **Primary controls:** Email/password inputs, show-password toggle, Sign In, Forgot Password, navigation back.
- **Navigation:** Successful sign-in continues through guest-history choice if needed, then returns to Profile or the originally requested feature.
- **States and feedback:** Failed credentials show an error. Practice is not blocked by a sign-in failure.

### Sign Up (`sign-up.svg`)

**Supports user flow(s):** 7

- **Purpose:** Collect the details needed to register an account.
- **Information shown:** Username, email, password, confirm password, and password requirements.
- **Player actions/input:** Enter information and submit registration, or return to Sign In.
- **Primary controls:** Four inputs, submit control, password requirement indicators, Back/Sign In entry.
- **Navigation:** Successful registration leads to Check Email.
- **States and feedback:** Username length and allowed characters are validated, uniqueness is case-insensitive, and passwords must match; unmet requirements show an error or unmet indicator.

### Check Email (`check-email.svg`)

**Supports user flow(s):** 7

- **Purpose:** Tell a new user what to do after registration.
- **Information shown:** Email verification instructions and next step toward sign-in.
- **Player actions/input:** Continue to Sign In or go back.
- **Primary controls:** Instruction text and Sign In/Back navigation.
- **Navigation:** Returns to the Sign In flow.
- **States and feedback:** The player completes email verification and sign-in before accessing account-only features.

### Password Reset (`password-reset.svg`)

**Supports user flow(s):** 7

- **Purpose:** Allow an account holder to request password recovery.
- **Information shown:** Email entry and explanation of the reset request.
- **Player actions/input:** Enter an email, request a reset, or return to Sign In.
- **Primary controls:** Email input, Send Reset Link, and Return to Sign In.
- **Navigation:** Returns to Sign In after the request or when cancelled.
- **States and feedback:** Confirmation or error feedback tells the player whether the request was accepted; no password is changed directly on this screen.

### Guest History Choice (`guest-history-import.svg`)

**Supports user flow(s):** 7

- **Purpose:** Allow a signed-in player to decide what happens to existing guest Practice history.
- **Information shown:** Number of saved guest games and victories, with the choices to keep or discard that history.
- **Player actions/input:** Keep My Practice History or Discard Guest History.
- **Primary controls:** Two choice controls.
- **Navigation:** Keep continues to Profile/original destination; Discard opens an additional confirmation.
- **States and feedback:** Guest history stays saved unless the player confirms they want to discard it.

### Discard Guest History (`discard-guest-history.svg`)

**Supports user flow(s):** 7

- **Purpose:** Confirm an irreversible choice to discard local guest Practice history.
- **Information shown:** Warning that the guest history will be removed if confirmed.
- **Player actions/input:** Cancel or confirm the discard.
- **Primary controls:** Keep My Practice History and Permanently Discard History buttons.
- **Navigation:** Cancel returns to Guest History Choice; confirmation continues to Profile/original destination.
- **States and feedback:** Cancel preserves the guest history; Confirm deliberately discards it.

### Account Settings (`account-settings.svg`)

**Supports user flow(s):** 8, 9

- **Purpose:** Give a signed-in player a place to manage account details and security actions.
- **Information shown:** Account information, security options, username editing, Sign Out, and Delete Account.
- **Player actions/input:** Choose an account action or return to Settings.
- **Primary controls:** Edit Username, Sign Out, Delete Account, and Back entries.
- **Navigation:** Edit Username and Delete Account open in a stack; Sign Out returns to Guest Profile.
- **States and feedback:** Delete Account is not a direct action on the main Profile. Account-only actions require a signed-in state.

### Edit Username (`edit-username.svg`)

**Supports user flow(s):** 9

- **Purpose:** Allow a player to change their public username within account rules.
- **Information shown:** Current username, new username field, and once-per-30-days restriction.
- **Player actions/input:** Enter a new username, save, or go back.
- **Primary controls:** Username input, Save and Back controls.
- **Navigation:** Returns to Account Settings after saving or cancelling.
- **States and feedback:** Invalid or unavailable names show feedback. Changes are blocked while the 30-day restriction applies.

### Delete Account (`delete-account.svg`)

**Supports user flow(s):** 9

- **Purpose:** Require a clear confirmation before permanent account deletion.
- **Information shown:** Warning that the account and associated saved progress will be permanently deleted.
- **Player actions/input:** Confirm deletion or cancel.
- **Primary controls:** Explicit Confirm and Cancel controls.
- **Navigation:** Cancel returns to Account Settings; confirmed deletion returns to the guest experience.
- **States and feedback:** Cancel leaves the account unchanged. After confirmed deletion, the app returns to the guest experience.

## 6. Settings and Customization

### Settings (`settings.svg`)

**Supports user flow(s):** 8, 9

- **Purpose:** Let guests and signed-in players control gameplay behaviour and open related settings.
- **Information shown:** Gameplay control methods, sound and vibration, colours, Ranked reminders, Coaching preferences, and account options.
- **Player actions/input:** Choose a control method, toggle sound/vibration, or open another settings area.
- **Primary controls:** Control selection, switches, Colours/Ranked Notifications/Account Settings entries, Back.
- **Navigation:** Opened from Profile; other pages use stack navigation. Guests can access general settings, while account-only actions require sign-in.
- **States and feedback:** Current selections are visible and changes are saved automatically. Gameplay uses the selected control settings.

### Colour Customization (`colour-customization.svg`)

**Supports user flow(s):** 8

- **Purpose:** Let players change the game's appearance.
- **Information shown:** Adjustable colours for numbers, flags, mines, covered/revealed squares, backgrounds, borders and buttons; small board preview.
- **Player actions/input:** Choose colours, reset them, or return to Settings.
- **Primary controls:** Colour selectors, live board preview, Reset to Defaults, Back.
- **Navigation:** Opens from Settings and returns there.
- **States and feedback:** The preview reflects selected colours. Reset to Defaults restores the original palette.

### Ranked Notifications (`ranked-notifications.svg`)

**Supports user flow(s):** 8

- **Purpose:** Allow players to control reminders for paused Ranked matches.
- **Information shown:** Notification option for an approaching end to a paused Ranked game's total time allowance.
- **Player actions/input:** Enable or disable the reminder, review notification permissions, or return to Settings.
- **Primary controls:** Reminder switch, Review Notification Permissions, and Back.
- **Navigation:** Opens from Settings and returns there. The permissions option leads to the device notification settings where supported.
- **States and feedback:** The reminder preference is visible. Enabling it does not extend the 24-hour Ranked pause allowance, and notifications require device permission.

## Shared interface behaviour

- **Five tabs:** Play, Statistics, Coaching, Leaderboard, and Profile provide main navigation. Settings is opened through Profile, not as a sixth tab.
- **Full-screen gameplay:** Gameplay hides the bottom tabs to give the board more room. Game results, account pages, history details, and settings use stack navigation where appropriate.
- **Practice and Ranked pauses differ:** A paused Practice board remains visible and inactive; a paused or backgrounded Ranked board is hidden.
- **Access rules:** Practice and local Practice statistics remain available to guests. Ranked and Coaching require sign-in and internet. The public Master leaderboard is viewable without an account.
- **Confirmation for destructive actions:** Replacing an unfinished Practice game, discarding guest history, and deleting an account require clear choices before data is lost.
- **Ranked gameplay state:** Active Ranked matches use the full-screen Minesweeper gameplay interface with Intermediate difficulty. The Ranked-specific state adds authenticated session handling, result checking, hidden board on interruption, and a cumulative 24-hour paused-time allowance.
- **Coaching limits:** Each account has 10 requests per UTC day. The interface shows how many remain. Failed requests without a usable answer do not count, and Ranked has no live AI hints.
