# MindSweeper — Phase 2 UI Design Prompt

I'm working on a solo mobile app project for my Software Development program at SAIT. The app is called MindSweeper. It's a Minesweeper game with offline Practice Mode, competitive Ranked Mode, statistics, optional AI coaching, accounts, and a leaderboard.

Right now I'm working on the UI mockups. I want to see what the different screens are going to look like before I start building the app. For this prompt, I only want the mockups. 

I've spent time planning how I want the app to work, so please follow the requirements below rather than changing features just because a different design might look nicer. My main goal is to make something that feels like an actual game people would enjoy using, not a generic school-project app.

## 1. Overall design

I'd like MindSweeper to have a dark, minimalistic visual style, mostly black and charcoal with a strong red accent. It should look modern and polished but still be easy to read and use on a mobile device. It should feel competitive, especially for the Ranked Mode, but I don't want it covered in unnecessary decorations or giant cards with almost no information in them.

Please use this general visual guideline:

- Mostly black and charcoal backgrounds, with red as the main accent colour.
- Approximate colour palette: background `#0D0D0D`, surfaces `#171717` and `#252525`, borders `#2B2B2B`, primary text `#F5F5F5`, secondary text `#B1B1B1`, muted text `#909090`, and red accent `#E83B45`.
- Roboto Condensed for prominent headings and Roboto for normal text, labels, and buttons. Use bold weights where they actually help the hierarchy.
- Clean, recognizable icons with consistent sizing and alignment. Settings should look like a gear.
- Rounded panels and buttons, but no excessive nesting or empty space. Make the layout readable on a phone.

### Branding I worked on before the mockups

I also worked on a separate branding design before starting these screens. I wanted the logo and rank badges figured out first so I could use the same designs throughout the app instead of making something different for each screen.

The branding includes three SVG files:

- **`mindsweeper-mark.svg`** — the main MindSweeper logo. It has a red mine symbol next to the name in bold white text. The mine has a round red centre with short spikes extending outward, two shades of red, and a couple of small details inside. I'd also like a smaller version of just the mine icon for the Play tab.
- **`master-emblem.svg`** — a purple badge for Master, which is the highest rank. I took the purple Ace rank badge from Rainbow Six Siege as inspiration, especially the layered shapes and colours, but I wanted an original design for MindSweeper. 
- **`gold-ii-emblem.svg`** — a gold shield with a Roman numeral II in the middle. I chose Gold II as an example because it's around the middle of the ranking system. This badge should appear whenever the example player is Gold II, especially in the Ranked Lobby.

The main logo is for MindSweeper as a whole, while the rank badges are for the player's actual rank. I don't want the main logo used in place of a rank badge. I provided the finished SVGs, please use those files instead of redesigning them.

Use a typical phone canvas for the mockups. I understand the app will eventually need to adapt to different screen sizes; the mockups should still make mobile spacing and navigation obvious. Everything should feel like one consistent product.

## 2. Navigation and Play screen

I want the app to open straight to the Play tab with Practice Mode selected. Someone should be able to open the app and start playing Practice without having to sign in or go through an introduction screen.

There are five tabs along the bottom:

1. **Play** — Practice and Ranked Mode.
2. **Statistics** — Practice and Ranked stats, plus game history.
3. **Coaching** — AI chat and feedback after a game.
4. **Leaderboard** — Master players and the player's own global rank.
5. **Profile** — guest or signed-in profile, with Settings inside.

I want the tabs to look the same across the main screens. When someone starts a Minesweeper game, though, I'd rather have a full-screen game view so there's more room for the board. Other screens, like signing in or confirming that you want to replace a saved game, can open separately.

## 3. Practice Mode mockups

Practice Mode should be available offline and without an account, so I want that to be obvious on the Play screen.

The three difficulties are:

| Difficulty | Board size | Mines |
|---|---|---:|
| Beginner | 9 × 9 | 10 |
| Intermediate | 16 × 16 | 40 |
| Expert | 16 × 30 | 99 |

For the game screen, I'd like the board to take up most of the space. It should still have a timer, a mine/flag counter, pause and restart controls, and a way to zoom or move around larger boards.

The controls I'm planning are tapping a square to reveal it, holding it to place a flag, and an optional Flag Mode. Players can also tap an already revealed number to open nearby squares when the right number of flags has been placed. I'd like the mockup to show these controls without making the screen too crowded.

Please make the example boards look like real Minesweeper games. The numbers should match the mines around them, flags should make sense, and there shouldn't be more flags than mines. The first square opened should be safe and reveal an empty area.

I'd also like the following Practice screens:

- **Resume Game:** If someone has an unfinished game, show a card on the Play screen so they can continue it.
- **Replace Saved Game:** If they try to start a new game while one is saved, show a simple confirmation asking if they want to replace it.
- **Paused Game:** Keep the Practice board visible, but show that the timer is stopped and the player can't make moves until they resume.
- **Victory and Game Over:** Show the result, time, difficulty, moves, and flags, with buttons for **Play Again**, **Review Final Board**, and **Return to Play**. I'd also like an option to ask for AI analysis after a finished game.

## 4. Ranked Mode mockups

Ranked Mode requires an account and an internet connection. I want separate screens explaining this when someone isn't signed in or is offline. The account screen should have Sign In, Create Account, and Cancel. The offline screen should mention that Practice Mode is still available.

For now, Ranked will only use Intermediate (16 × 16, 40 mines). I don't need other difficulty choices on the Ranked screen.

The Ranked Lobby is one of the screens I care about most. I'd like it to show the player's rank badge, rank name, RP, progress toward the next division, global position, and a clear Start Match button. Use my Gold II badge for the example player.

I'd also like a separate screen for placement matches. New Ranked players need to complete 10 placement games before their starting rank is shown, so this screen could show something like 5/10 placements completed.

For the examples in the mockups, the ranks go from Bronze, Silver, Gold, Platinum, Diamond, then Master. The first five ranks have divisions I, II, and III. Each division covers 100 RP, with Master starting at 1,500 RP. A Gold II player with 740 RP could show 40/100 RP toward Gold III.

The Ranked Results screen should clearly show whether the player won or lost, along with their updated RP. For example, I like YOUR RP: 760 with +20 RP after a win. Please make sure the RP matches between screens.

## 5. Statistics and game history

I'd like Practice and Ranked statistics on separate screens. I don't want everything mixed together.

For Practice, show total games, wins, losses, win percentage, and best times. Include difficulty choices for All, Beginner, Intermediate, and Expert. Guests should still be able to see their Practice stats.

For Ranked, show games played, wins, losses, win rate, current rank and RP, global position, best Ranked time, and the player's highest rank displayed. The example I'd like to use is All-Time Highest Ranked: GOLD III (820), while their current rank is Gold II (740 RP). I'd also like the Best Ranked Time and All-Time Highest Ranked sections to be bolded and placed in a separate card so they stand out more.

I want a Game History screen where players can see previous games and open one to view its details. The Game Details screen should show the finished board, including the numbers, mines, flags, and incorrect flags with a red X. Add controls to zoom or centre the board if it doesn't fit on the screen.

## 6. AI Coaching mockups

I want the Coaching section to help players learn Minesweeper, not just give them an answer. I think it would be useful to have a main Coaching screen with options to Ask a Question or Analyze a Completed Game.

For the chat mockup, I'd like to see an example conversation about Minesweeper strategy. A 1-2-1 pattern would be a good example, as long as the explanation and the board example are actually correct. If the AI says a square is safe or has a mine, there needs to be enough information in the example to know that for sure.

The Coaching Report screen should show what the player did well, what they could improve, and an option to look at the final board.

Coaching needs an account and internet, so those messages should be clear when needed. Players have 10 AI requests per day, resetting at 00:00 UTC, so I'd like the available requests to be visible. Please also make a screen for when they've used all 10 requests.

I don't want live AI hints shown during Ranked games.

## 7. Leaderboard mockups

I want a Master Leaderboard that looks like something from a competitive game, rather than just a plain list of names. Use my purple Master emblem at the top and show player positions, usernames, and RP in a way that's easy to read.

Only current Master players should appear in the public leaderboard list. Players in lower ranks should still be able to see their own global position after completing placements. For example, a Gold II player could have a global rank of #428, while a Bronze II player might be #1,283. These players shouldn't appear on the public Master leaderboard, but they should still be able to see their own ranking.

Guests should be able to view the leaderboard, but where their personal rank would be, show “Sign in to see your global position.” Someone still doing placements should see their placement progress instead.

Please use the usernames, MooseByte, CozyComet, NotMyMine, and FrostByte on the leaderboard during the mockup creation. 

## 8. Accounts and Profile mockups

I want a Guest Profile and a separate signed-in Profile. Guests can play Practice, but they'll need to sign in to use Ranked and AI Coaching.

The signed-in Profile should show the player's username, rank badge, global position, and a few useful stats. Keep Edit Username and Delete Account under **Profile → Settings → Account**, instead of putting them directly on the Profile screen.

For Sign Up, I'd like username, email, password, and confirm password on the same screen. Usernames should be 3–20 characters, use letters, numbers, or underscores, and be unique even if the capitalization is different. A player can change their username once every 30 days, so that should be explained on the Edit Username screen.

The password requirements should be shown in a short list under the password field: at least 8 characters, one uppercase letter, one lowercase letter, and one number. If the password field is empty in the mockup, show an X beside each requirement. Use simple placeholders like Enter Password.

For Sign In, include a password visibility button, Forgot Password, and the line “No account needed for practice.” I'd also like screens for checking an email after registration and resetting a password.

If someone has Practice history saved as a guest and then signs in, show a screen asking if they want to Keep My Practice History or Discard Guest History. If they choose to discard it, I want a second confirmation screen so they don't delete it by accident.

For the Delete Account screen, make it clear that the account and its saved progress will be deleted, and ask for confirmation. I don't want that action to look like a regular Settings option someone could tap by mistake.

## 9. Settings mockups

Settings should be available to guests and signed-in players. I'd like the main Settings screen to be organized into a few sections instead of one giant list.

The settings I'd like shown are:

- **Gameplay:** long-press to flag or use Flag Mode, and tapping numbers to open nearby squares or using a separate chord button.
- **Sound and Vibration:** separate switches for each.
- **Colour Customization:** a separate screen where players can change the colours of numbers, flags, mines, squares, backgrounds, borders, and buttons. I'd like a small board preview and a **Reset to Defaults** button. 
- **Ranked Notifications:** a screen with an option to get a reminder when the Ranked pause allowance is nearly used up.
- **Coaching Preferences:** a place for settings related to the type and amount of coaching explanations.
- **Account:** username, password or security options, sign out, and delete account.

## 10. Screens I want designed

I'd like **36 SVG mockups** in total. Please use informative filenames that explain what each screen is.

**List of All screens I need**

**Play and gameplay:**
`play.svg`, `resume-game.svg`, `replace-saved-game.svg`, `practice-game.svg`, `practice-paused.svg`, `victory.svg`, `game-over.svg`

**Statistics:**
`practice-statistics.svg`, `ranked-statistics.svg`, `game-history.svg`, `game-details.svg`

**Ranked:**
`ranked-lobby.svg`, `ranked-placements.svg`, `ranked-results.svg`, `account-required.svg`, `ranked-offline.svg`

**Coaching:**
`coaching.svg`, `coaching-chat.svg`, `coaching-report.svg`, `ai-request-limit.svg`

**Leaderboard:**
`leaderboard.svg`, `guest-leaderboard.svg`

**Accounts and Profile:**
`guest-profile.svg`, `profile.svg`, `sign-in.svg`, `sign-up.svg`, `check-email.svg`, `password-reset.svg`, `guest-history-import.svg`, `discard-guest-history.svg`, `edit-username.svg`, `account-settings.svg`, `delete-account.svg`

**Settings:**
`settings.svg`, `colour-customization.svg`, `ranked-notifications.svg`

## 11. Final check

Please give me the actual editable SVG mockups, not just pictures of what they might look like. I'd like the files to be organized so I can open each screen and request changes if something doesn't look right. I only want the mockup files for this prompt, using my existing branding where it belongs.

Before calling them finished, please check that the text fits on each screen, the buttons and icons are consistent, and the right rank badges are being used. I also want the Minesweeper boards and the example Ranked numbers to make sense across the different screens.

If something is unclear, let me know instead of changing an important part of the design. I want to review the mockups first before we move on to building the application.
