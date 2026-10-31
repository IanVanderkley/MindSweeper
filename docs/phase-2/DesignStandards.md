# MindSweeper — Phase 2 Design Standards

## Overview

This document defines the visual standards for MindSweeper so the screens stay consistent when the app is developed. The standards are based on the existing [interface scope](InterfaceScope.md), [user flows](UserFlows.md), and [screen mockups](../../design/mockups/).

MindSweeper uses a dark interface with red accents. The game board remains the main focus, while the surrounding screens use the same fonts, colours, buttons, cards, and navigation style. These are the default designs for Android and iOS.

## 1. Typography

MindSweeper uses Roboto Condensed for prominent headings and Roboto for regular interface text. This keeps titles recognizable while making instructions, buttons, and game information easy to read. I used YouTube's colours and typography as inspiration for MindSweeper's design, adapting them to fit my application.

| Text role | Font | Size | Weight | Where it is used |
|---|---|---:|---:|---|
| Screen title | Roboto Condensed | 27 | 700 (bold) | Main title at the top of a screen |
| Feature or result heading | Roboto Condensed | 21–29 | 700 (bold) | Victory, Game Over, rank displays, and confirmation headings |
| Section heading | Roboto | 16 | 700 (bold) | Cards and sections such as Gameplay or Game Summary |
| Body text | Roboto | 13–14 | 400 (regular) | Descriptions, explanations, and general information |
| Field or setting label | Roboto | 13–14 | 600–700 | Form labels and settings |
| Button text | Roboto | 12–14 | 700 (bold) | Main buttons, secondary buttons, and selectors |
| Supporting text | Roboto | 11–12 | 400 (regular) | Hints, requirements, and smaller status messages |
| Bottom-tab label | Roboto | 11 | 700 (bold) | Play, Stats, Coach, Ranks, and Profile |
| Board numbers | Roboto | Scales with the tile; around 14 in the current game mockups | 700 (bold) | Revealed Minesweeper numbers |

These sizes reflect the current mockups. In React Native, font sizes will use the corresponding logical units and adapt as needed for device size and accessibility settings. The same text roles will be used throughout the app rather than choosing a new font or size for every screen.

## 2. Colour Palette

The following colours are used across the current mockups. Hex codes define the default palette; players can change supported colours in Colour Customization.

| Role | Colour | Hex code | Usage |
|---|---|---|---|
| Main background | Dark | `#0D0D0D` | Page background |
| Cards and navigation | Dark grey | `#171717` | Cards, panels, and bottom navigation |
| Raised surface | Grey | `#252525` | Revealed board tiles and additional surfaces |
| Border | Dark grey | `#2B2B2B` | Card outlines and separators |
| Primary text | Off-white | `#F5F5F5` | Headings, values, and button text |
| Secondary text | Light grey | `#B1B1B1` | Descriptions and supporting information |
| Muted text and icons | Grey | `#909090` | Inactive tabs and quieter helper text |
| Main accent | Red | `#E83B45` | Selected tabs, icons, outlines, and switches |
| Primary button | Dark red | `#C92E38` | Main actions such as Start Game |
| Selected control background | Dark red-grey | `#3D2327` | Selected mode and settings choices |
| Unselected control background | Grey | `#292929` | Unselected choices and secondary buttons |
| Success | Green | `#65D9A2` | Positive RP changes and success information |
| Warning or gold highlight | Gold | `#F2C66D` | Gold rank details and highlighted warnings |
| Error or loss highlight | Light red | `#FF727B` | Game-over and error emphasis |
| Destructive action | Deep red | `#8D2932` | Permanent deletion button |

The main accent red is also used to show which tab or control is selected. Success, warning, and error messages will use both text and colour so the meaning does not depend on colour alone. Primary buttons use off-white text against the dark red fill.

### Minesweeper board colours

The game uses a slightly different group of greys to make covered and revealed squares easy to distinguish:

| Board element | Default colour |
|---|---|
| Covered square | `#383838` |
| Revealed square | `#252525` |
| Number 1 | `#69B2FF` (blue) |
| Number 2 | `#65D9A2` (green) |
| Number 3 | `#F19A8B` (coral) |
| Number 4 | `#AD8BFF` (purple) |
| Flags and incorrect-flag highlights | Red or pink accents consistent with the game mockups |

The current mockups show numbers 1–4. Higher numbers will follow the same rule of using distinct, readable colours. The chosen colours will also apply to the read-only board in Game Details.

## 3. Themes and Colour Customization

The default MindSweeper theme is dark. A separate light-mode theme is not part of the current design. Players will be able to change number colours, flags and mines, covered and revealed squares, backgrounds, and accent colours through Settings → Colour Customization. The screen includes a small live board preview so players can see their changes before returning to the game. Reset to Default Colours restores the original palette.

## 4. Icons and Branding

MindSweeper will use @expo/vector-icons for navigation, buttons, and settings, as outlined in the architecture. Icons will follow a consistent style and will be very similar in size (20-24px). The five navigation tabs will each have their own icon. The selected tab will use red (#E83B45), while the others will use grey (#909090).

MindSweeper also has its own branding and rank artwork. These files will be reused instead of redrawing the same symbols on different screens:

- [MindSweeper mark](../../design/assets/branding/mindsweeper-mark.svg)
- [Master emblem](../../design/assets/branding/master-emblem.svg)
- [Gold II emblem](../../design/assets/branding/gold-ii-emblem.svg)

I plan to use AI to create the remaining rank emblems in the same style as the existing designs 

## 5. Layout and Shared Components

The mockups use a 390 × 844 phone layout (based on the iPhone 14, which I use) as a design reference. The finished app will adapt to different screen sizes rather than relying on those exact dimensions.

| Element | Standard |
|---|---|
| Screen margins | Around 20 units on the left and right |
| Card padding | Around 16 units inside cards |
| Spacing | Consistent gaps of about 8, 12, or 16 units depending on the content |
| Cards | `#171717` fill, `#2B2B2B` border, and approximately 12-unit rounded corners |
| Main buttons | `#C92E38` fill, off-white text, approximately 9-unit rounded corners |
| Selectors | About 8-unit rounded corners; selected choices use the red outline and darker red background |
| Bottom navigation | Dark surface with five evenly spaced icons and labels |
| Dividers | Thin `#2B2B2B` lines where sections need separation |

The same button styles, cards, headings, and spacing will be reused across Practice, Ranked, Coaching, Statistics, and Settings. Important actions will be large enough to tap comfortably, but not so large to overlap on smaller screens. 

## 6. States and Feedback

The app will communicate changes using the same visual rules across screens:

- Selected modes, tabs, and settings use the red accent and a visible selected state. Unselected options use grey.
- Disabled controls will appear different from active controls so players can easily tell which options are unavailable.
- Success messages will use green, while errors, losses, and unavailable features will display clear messages with appropriate warning or error colours.
- Confirmations for replacing a saved game, discarding guest history, and deleting an account keep the destructive action visually distinct from Cancel.
- Forms show password requirements and validation feedback near the related fields. Loading and error states explain what is happening without changing the overall layout.
- Practice pauses with the board still visible but unable to accept reveal or flag actions. Ranked hides the board when paused or when the player switches away from the app.
- The same read-only board presentation is used when reviewing completed games.

## 7. Keeping the Design Consistent

The [36 screen mockups](../../design/mockups/) are the visual reference for implementation. When screens are built in React Native, shared colours, text styles, spacing, buttons, and cards will be defined in reusable styles.

Any changes to the default design will be reflected in these standards so new screens remain consistent with the rest of MindSweeper.
