# MindSweeper — Phase 2 User Flows

## Overview

MindSweeper is a mobile Minesweeper game with offline Practice Mode, online Ranked Mode, statistics, AI Coaching, a leaderboard, and player accounts. The flows below explain what players are trying to do, the choices they make, and how they move through the app.

The two main user stories cover playing Practice Mode without an account or internet connection and signing in to play Ranked matches that count toward a player's rank. The remaining flows cover saved games, statistics, Coaching, the leaderboard, settings, and account management.

## How navigation works

MindSweeper will have five main tabs: Play, Statistics, Coaching, Leaderboard, and Profile. The app will open on the Play tab with Practice Mode already selected. Settings will be accessed through Profile instead of having its own tab.

Navigation will work in three ways:

- **Tab navigation:** Players switch between the five main areas of the app.
- **Stack navigation:** Players open screens such as gameplay, game details, Sign In, or Settings, then use Back to return.
- **Confirmation screens or dialogs:** The app asks for confirmation before actions such as replacing a saved game, discarding guest history, or deleting an account.

Each linked diagram shows the screens involved and the paths a player can take between them.

## Flow 1 — Start and finish a Practice game

**User story:** As a player, I want to play Minesweeper without having to create an account or connect to the internet.

**Starting point:** The player opens MindSweeper and lands on the Play tab. Practice Mode is already selected.

[View user flow diagram](userFlowDiagrams/Start%20and%20finish%20a%20Practice%20game.png)

The Play screen will offer three difficulties: Beginner (9 × 9, 10 mines), Intermediate (16 × 16, 40 mines), and Expert (16 × 30, 99 mines). After choosing a difficulty and starting Practice, the player will enter a full-screen game.

During a game, the player will see the board, timer, mine/flag counter, and controls. Tapping a square will reveal it, while holding a square will place a flag. Players can also use an optional Flag Mode. Larger boards will have zoom and movement controls. The first revealed square will be safe and open an empty area. The player wins after revealing every safe square, even if some mines are not flagged.

After a win or loss, the result screen will show the difficulty, time, moves, and flags. The player can play again, review the final board, return to Play, or request AI analysis. Selecting AI analysis will lead to Coaching, which requires sign-in. Practice Mode will remain available to guests without an account.

**Navigation used:** Play tab → full-screen game in a stack → result screen → Play, Game Details, or Coaching.

## Flow 2 — Pause, resume, or replace a saved Practice game

**User story:** As a player, I want to leave an unfinished Practice game and continue it later without losing my progress.

**Starting point:** The player is in a Practice game, or returns to the Play tab with an unfinished game saved.

[View user flow diagram](userFlowDiagrams/Pause%2C%20resume%2C%20or%20replace%20a%20saved%20Practice%20game.png)

When a Practice game is paused, the board will remain visible, but the timer will stop and the player will not be able to reveal or flag squares. If the player leaves the app or closes it, the unfinished game will be saved and paused automatically.

When the player returns, the Play tab will show a Resume Game option. If they start a new Practice game instead, a confirmation will appear before the saved game is replaced. Cancel will keep the unfinished game, while Replace will start a new game and remove the previous session.

**Navigation used:** Full-screen game stack, Play tab, and a confirmation screen/dialog for replacing a saved game.

## Flow 3 — Enter Ranked, complete placements, and see a result

**User story:** As a player, I want to sign in and play Ranked Minesweeper so my results count toward my rank.

**Starting point:** The player selects Ranked from the Play tab.

[View user flow diagram](userFlowDiagrams/Enter%20Ranked%2C%20complete%20placements%2C%20and%20see%20a%20result.png)

Ranked Mode will require both a signed-in account and an internet connection. Players who are not signed in will see Sign In, Create Account, and Cancel options. Players who are offline will see a message explaining that Ranked requires internet access and that Practice is still available.

Every Ranked match will use Intermediate difficulty (16 × 16, 40 mines). New players will complete 10 placement matches before seeing their starting rank. The placement screen will show their progress, such as 5/10 completed. After placements, the Ranked Lobby will display their rank badge, rank name, RP, progress to the next division, global position, and a Start Match button.

After a match ends, the result will be checked before any RP change is finalized. The results screen will show the win or loss and any RP change. The player can then start another match or return to Ranked. RP will remain hidden until all 10 placement matches are complete.

If a Ranked match is interrupted, the board will be hidden while the game is paused or in the background. Unlike Practice, a Ranked match will have a total allowance of 24 hours of paused time.

**Navigation used:** Play tab → an account/offline decision screen, or Ranked Lobby/Placements in the stack → gameplay → results → Ranked.

## Flow 4 — Check statistics and review a previous game

**User story:** As a player, I want to see how I'm improving and look back at games I've already played.

**Starting point:** The player selects the Statistics tab.

[View user flow diagram](userFlowDiagrams/Check%20statistics%20and%20review%20a%20previous%20game.png)

Practice Statistics will display games played, wins, losses, win percentage, and best times. Players can filter these statistics by All, Beginner, Intermediate, or Expert. Guests will also have access to their Practice statistics.

Ranked Statistics will display games played, wins, losses, win rate, current rank and RP, global position, and best Ranked time. Best Ranked Time and All-Time Highest Ranked will have separate cards.

From Game History, the player can select a completed game to review its final board. The Game Details screen will show revealed numbers, mines, flags, and incorrectly placed flags. This screen is for reviewing the game, so the player cannot make more moves. Zoom and centre controls will be available for viewing the board.

**Navigation used:** Statistics tab → mode selection → Game History and Game Details in a stack → Back.

## Flow 5 — Ask the AI Coach a question or review a finished game

**User story:** As a player, I want help understanding Minesweeper patterns and feedback on games I've completed.

**Starting point:** The player opens the Coaching tab, or chooses AI analysis after finishing a Practice game.

[View user flow diagram](userFlowDiagrams/Ask%20the%20AI%20Coach%20a%20question%20or%20review%20a%20finished%20game.png)

The Coaching screen will offer two options: Ask a Question and Analyze a Completed Game. Players can type a question and receive an explanation, or select a completed game for analysis. They can also reach game analysis directly from a finished Practice game.

A completed-game report will show what the player did well, what they could improve, and an option to review their final board. AI requests will only be used when a player submits a question or requests an analysis. Finishing a game will not automatically use a request.

Coaching will require a signed-in account and an internet connection. Each player will have 10 AI requests per day, with the limit resetting at 00:00 UTC. The Coaching screen will show the number of requests remaining. Players who are signed out, offline, or out of requests will see a message explaining why they cannot continue. Live AI hints will not be available during Ranked games.

**Navigation used:** Coaching tab → Chat or Report in the stack; a finished Practice result can also lead into the report flow.

## Flow 6 — View the Master leaderboard and personal global rank

**User story:** As a player, I want to compare myself with other players, even if I haven't reached Master.

**Starting point:** The player selects the Leaderboard tab.

[View user flow diagram](userFlowDiagrams/View%20the%20Master%20leaderboard%20and%20personal%20global%20rank.png)

The public leaderboard will list current Master players only. Each row will show the player's position, username, and RP. The Master emblem will appear near the top of the page.

Players below Master will not appear on the public Master leaderboard. However, signed-in players who have finished placements will still see their own global position. This personal global rank is separate from the public Master list.

Guests will be able to view the Master leaderboard, but the personal ranking area will say “Sign in to see your global position.” Signed-in players who have not finished placements will see their placement progress instead of a global position.

**Navigation used:** Leaderboard tab, with Sign In opening in a stack if the guest chooses to sign in.

## Flow 7 — Create an account, sign in, and decide what happens to guest history

**User story:** As a guest, I want to create an account to use Ranked and Coaching, while choosing what happens to my Practice history.

**Starting point:** The player opens the Guest Profile, tries to enter Ranked, or chooses a feature that requires an account.

[View user flow diagram](userFlowDiagrams/create-acount%2Csign-in%2Cguest-history.png)

The Sign Up screen will include username, email, password, and confirm password fields. Usernames must be 3–20 characters long, using only letters, numbers, or underscores, and must be unique regardless of capitalization. The password requirements will appear below the password field: at least 8 characters, one uppercase letter, one lowercase letter, and one number. When no password has been entered, all requirements will be marked as unmet. Invalid entries, including a taken username or mismatched passwords, will show an error and prevent registration.

The Sign In screen will include an Enter Password field, a show/hide password button, a Forgot Password option, and the message “No account needed for practice.” After signing up, the player will see a Check Email screen with the next steps. The Password Reset screen will allow the player to request a password reset.

If the player has saved Practice history as a guest, they will be asked to choose Keep My Practice History or Discard Guest History after signing in. Choosing Discard Guest History will open a second confirmation, with the option to Cancel. Guest history will only be discarded after the player confirms. The player will then reach their signed-in Profile or the feature they originally selected.

**Navigation used:** Profile tab or account-required screen → Sign In/Sign Up in a stack → guest history decision/confirmation → signed-in Profile or the requested feature.

## Flow 8 — Change game settings and colours

**User story:** As a player, I want to change the controls and colours so the game is comfortable for me to use.

**Starting point:** The player opens Profile, then Settings.

[View user flow diagram](userFlowDiagrams/Change%20game%20settings%20and%20colours.png)

Settings will be available to both guests and signed-in players. The main Settings screen will include sections for gameplay controls, sound, vibration, colours, Ranked notifications, Coaching preferences, and account options.

The Gameplay section will offer long-press flagging or Flag Mode, along with chording by tapping a revealed number or using a separate Chord button. Sound and vibration will each have their own switch.

The Colour Customization screen will let players change the colours of numbers, flags, mines, covered and revealed squares, backgrounds, borders, and buttons. A small board preview will show the selected colours, and Reset to Defaults will restore the original colours. The Ranked Notifications screen will include a reminder option for when a paused Ranked game's time allowance is almost used up.

**Navigation used:** Profile tab → Settings in a stack → Colour Customization, Ranked Notifications, or Account Settings → Back.

## Flow 9 — Update account details or delete an account

**User story:** As a signed-in player, I want to manage my account and have a clear confirmation before making a permanent change.

**Starting point:** The signed-in player opens Profile → Settings → Account.

[View user flow diagram](userFlowDiagrams/Update%20account%20details%20or%20delete%20an%20account.png)

Account Settings will contain account information, security options, Sign Out, and Delete Account. Delete Account will be accessed through Account Settings.

The Edit Username screen will show the current username, a field for entering a new one, and a message explaining that usernames can only be changed once every 30 days. The Delete Account screen will explain that the account and saved progress will be permanently deleted. The player must confirm before deletion, while Cancel will return to Account Settings without making changes.

Signing out will return the player to the guest experience, where Practice Mode will still be available without an account.

**Navigation used:** Profile tab → Settings → Account Settings in a stack, with a confirmation screen for deletion.

