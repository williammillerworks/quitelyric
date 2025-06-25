# Quiet Lyric — Product Specification

**Version:** 0.1  
**Author:** William Miller  
**Date:** 2025-06-25  
**Platform:** Responsive Web Application (Mobile-first, Desktop-ready)

---

## Overview

Quiet Lyric is a browser-based, single-player music guessing game where users identify a song title based only on the first letters of its lyrics. The game is played once daily. Players buzz in to stop the letter stream and enter a guess. Correct answers reveal the full song metadata. Incorrect guesses impose a short delay before another attempt is allowed. The goal is to create a minimalist, habit-forming experience similar in spirit to Wordle.

---

## Gameplay Mechanics

### Daily Challenge
- One unique song per day.
- Only one playthrough per day per user/device.

### Lyric Letter Reveal
- Letters are revealed one by one, showing only the **first letter of each word** in a lyric snippet.
- Reveal pace: one letter every **1.2 seconds** (configurable).
- Lyric format is line-based and accurate to the original phrasing.

**Example:**

Original Lyric:  
`Let it be, let it be, let it be`  
Displayed As:  
`L I B, L I B, L I B`

### Buzz Mechanic
- A "BUZZ" button stops the reveal at any point.
- Player is prompted to guess the full **song title**.

### Guess Validation
- Case-insensitive and punctuation-agnostic matching.
- Input is validated against a normalized title list.

**If correct:**
- Song title and artist are revealed.
- Cover art, release year, and external link (Spotify/YouTube) are displayed.
- Game ends with success.

**If incorrect:**
- Input is disabled for **5 seconds**.
- Countdown is shown, lyric continues revealing.
- Player may guess again after the cooldown.

### End Conditions
- Game ends when the user guesses correctly, or the lyric snippet fully reveals.
- If time expires without a correct guess, the correct title is revealed with a failure message.

---

## Interface Design

### Layout Structure
- Mobile-first responsive UI.
- Clean vertical stack:
  - Header (date or future streak tracker)
  - Lyric display area (monospace or geometric font)
  - Buzz button or input field
  - Footer with brand and sharing options

### Game States
- `Idle` — Welcome and Start screen
- `Revealing` — Lyric stream is active
- `Guessing` — Buzz pressed, input field shown
- `Cooldown` — Guess locked for 5 seconds
- `Success` — Correct answer and metadata revealed
- `Failure` — Time expired, correct answer displayed

---

## Data Model

### Song Object Format

```json
{
  "date": "2025-06-25",
  "title": "Let It Be",
  "artist": "The Beatles",
  "lyric_snippet": "Let it be, let it be, let it be, let it be",
  "meta": {
    "year": 1970,
    "link": "https://open.spotify.com/track/abc123",
    "cover": "https://image.url"
  },
  "alt_titles": ["let it be", "LET IT BE", "Let it be"]
}
```

- `lyric_snippet`: Used to generate first-letter feed.
- `alt_titles`: Used for lenient matching on guess input.
- `meta`: Enhances end-of-game reveal.

---

## Input & Anti-Cheat

- Inputs normalized to lowercase and stripped of non-alphanumerics.
- Guess locked for 5 seconds after incorrect attempt.
- Only one game per user per day.
- Optional: prevent daily challenge spoilers in social posts.

---

## Share Feature

- On game end, display text for copy:
  - Example: `I guessed today's Quiet Lyric in 2 tries.`
- Link to homepage: `quiet.studio/lyric`
- Future (optional): image share with cover art and result count.

---

## Technical Overview

### Frontend
- React + Tailwind CSS
- State-driven reveal timer and input logic
- Components: `LetterStream`, `Buzzer`, `GuessInput`, `ResultDisplay`

### Backend / Data
- Static JSON or serverless function for daily song
- No authentication required for MVP
- Optional: Cloud-based song rotation or CMS

### Hosting
- Vercel or Netlify (Jamstack compatible)

---

## Scope

### MVP Includes
- Daily challenge logic
- Buzz + guess input flow
- Reveal & validation
- Success/failure end state
- Responsive UI
- Share text output

### MVP Excludes
- User accounts
- Audio playback
- Hints or clues
- Streak tracking
- Leaderboards

---

## Future Enhancements

- Daily streaks and profiles
- Difficulty levels (e.g., chorus-only vs. deep cuts)
- Audio clip mode (guess based on snippet)
- Shareable result images
- Anonymous stats (e.g., average guesses per day)

---

## Next Steps

1. Create design wireframes (mobile and desktop)
2. Define seed data for local prototyping
3. Implement core letter reveal + BUZZ mechanic
4. Prepare initial layout and UI elements
5. Write unit tests for input validation and title matching

---
