# Daily Progress Log

## Day 1 - Pomodoro Timer
**Date**: January 7, 2026
**Status**: Complete
**Category**: Utility
**Difficulty**: Easy
**Time**: ~1 hour

### What I Built
A Pomodoro timer with both CLI and browser versions. No dependencies, works offline.

### Build Notes
**CLI version:**
- Built core timer with countdown loop using `time.sleep()`
- Added progress bar with Unicode block characters
- Implemented macOS sound notification via `afplay`
- Added demo mode for quick testing (`--demo` flag)

**Web version:**
- Single HTML file, no dependencies, works offline
- Circular SVG progress ring
- Color-coded backgrounds (purple=work, green=short, orange=long)
- Web Audio API for sound (C-E-G chord chime)
- Browser notifications when timer completes
- Keyboard shortcuts (Space, R, S)

### Prompts Used
- Initial prompt to build the CLI version
- "What if it ran in a browser?" led to web version

### Lessons Learned
- Terminal escape codes (`\r`, `\033[K`) for in-place updates
- SVG `stroke-dasharray`/`stroke-dashoffset` for circular progress
- Web Audio API oscillators for generating tones
- CSS gradient transitions for smooth color changes

### Links
- GitHub: https://github.com/ilmych/day-01-pomodoro-timer

---

## Day 2 - Meeting Excuse Generator
**Date**: January 7, 2026
**Status**: Complete
**Category**: Fun
**Difficulty**: Medium
**Time**: ~2 hours

### What I Built
Spin-the-wheel excuse generator with AI-generated excuses and stylized image cards.

### Build Notes
- Two-step AI pipeline:
  1. Claude generates 50+ absurd excuses + believability ratings
  2. Gemini creates stylized image cards with text baked in
- All pre-generated, so app is instant (no runtime API calls)
- Spin wheel animation for drama
- Download or copy with one click

### Key Insight
Pre-generation is underrated. Generate everything upfront, store as static files. App loads instantly, costs nothing to run.

### Links
- GitHub: https://github.com/ilmych/day-02-meeting-excuse

---

## Day 3 - Text Cleaner
**Date**: January 8, 2026
**Status**: Complete
**Category**: Utility
**Difficulty**: Easy
**Time**: ~45 minutes

### What I Built
Web tool + Chrome extension for fixing garbage text from PDFs and emails.

### Build Notes
**Web tool:**
- Toggle checkboxes for each cleaning operation
- Live preview of cleaned text
- Copy to clipboard

**Chrome extension:**
- Manifest V3
- Context menu integration ("Clean Text")
- Popup for quick access
- Generated icon using Gemini AI

### Technical Challenge
Chrome's Manifest V3 service workers can't directly access clipboard. Routed through popup context where `document.execCommand('copy')` works.

### Prompts Used
```
"what's day 3? let's build day 3"
"yes! could we make it as a browser extension too?"
"nope, this is great. let's publish"
```

### Links
- Live: https://ilmych.github.io/day-03-text-cleaner/
- GitHub: https://github.com/ilmych/day-03-text-cleaner

---

## Day 4 - AI Flashcard Quiz
**Date**: January 8, 2026
**Status**: Complete
**Category**: Learning
**Difficulty**: Medium
**Time**: ~1 hour

### What I Built
Paste text, get AI-generated flashcards, quiz yourself.

### Build Notes
- Uses Gemini API with BYOK (Bring Your Own Key) pattern
- No backend needed—all client-side
- 3D flip animation (CSS transforms)
- Quiz mode with scoring
- Keyboard shortcuts: Space (flip), arrows (right/wrong), S (shuffle), R (restart)

### The Gemini Prompt
```
Extract 5-10 key facts, concepts, or important points from this text.
For each one, create a flashcard with:
- A clear, specific question
- A concise answer (1-2 sentences max)
Return as JSON array: [{"question": "...", "answer": "..."}, ...]
```

### Key Pattern: BYOK
Bring Your Own Key eliminates backend:
- No infrastructure to maintain
- No rate limits to manage
- No API costs
- User has full control

### Prompts Used
```
"what's day 4"
"yes, let's build it!"
"ooh i like it! do we need to add anything else to this?"
"yes let's publish it"
```

### Links
- Live: https://ilmych.github.io/day-04-flashcard-quiz/
- GitHub: https://github.com/ilmych/day-04-flashcard-quiz

---

## Day 5 - Deadline Dice
**Date**: January 8, 2026
**Status**: Complete
**Category**: Productivity
**Difficulty**: Medium-Hard
**Time**: ~2 hours

### What I Built
iOS app where you shake your phone to get a random deadline.

### Build Notes
**Stack:**
- Capacitor for iOS wrapping
- DeviceMotion API for shake detection
- Capacitor Haptics plugin for vibration
- Single HTML file for all UI/logic

**Core shake detection:**
```javascript
const total = Math.abs(acc.x) + Math.abs(acc.y) + Math.abs(acc.z);
if (total > 25) rollDice();
```

### Bugs Fixed
1. **Touch not working on iOS**: Added `-webkit-user-select: none` and proper event listeners
2. **Shake not triggering**: iOS 13+ requires motion permission on user interaction
3. **"Today" giving tomorrow**: Fixed date logic to respect midnight boundary
4. **Week/month boundaries**: Fixed to use calendar boundaries (Sunday midnight, month end)

### Prompts Used
```
"what's day 5"
"yes let's build it! can we do a mobile app where shaking your phone rolls the dice?"
"tapping doesn't work"
"cool, the shake doesn't work though"
"ok so i chose today, but it gives me deadlines that are tomorrow. we need a midnight cutoff"
"week should also be until midnight sunday, and month should be until midnight last day of current month"
"awesome! i think we can publish this"
```

### Key Lesson
Capacitor is underrated for simple mobile apps. Write a web app, wrap it, ship to iOS. No React Native or Flutter needed.

### Links
- GitHub: https://github.com/ilmych/day-05-deadline-dice

---

## Day 6 - Sleep Cutoff Calculator
**Date**: January 8, 2026
**Status**: Complete
**Category**: Health
**Difficulty**: Medium
**Time**: ~1.5 hours

### What I Built
iOS app that calculates when to stop caffeine, screens, and eating based on your wake time.

### Build Notes
- Capacitor iOS app
- Local notifications at cutoff times AND 30 min before
- Persistent settings in localStorage
- Dark theme matching iOS

### Links
- GitHub: https://github.com/ilmych/day-06-sleep-cutoff

---

## Summary Stats

| Day | App | Time | Deployed |
|-----|-----|------|----------|
| 1 | Pomodoro Timer | 1 hr | GitHub |
| 2 | Meeting Excuse | 2 hrs | GitHub |
| 3 | Text Cleaner | 45 min | GitHub Pages |
| 4 | Flashcard Quiz | 1 hr | GitHub Pages |
| 5 | Deadline Dice | 2 hrs | GitHub |
| 6 | Sleep Cutoff | 1.5 hrs | GitHub |

**Total**: ~8 hours for 6 complete apps
**Progress**: 6/30 (20%)
