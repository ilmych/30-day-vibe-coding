# Building 3 Apps in One Session with Claude Code

*Days 3-5 of 30 Days of Vibe Coding*

---

## The Setup

I'm building 30 apps in 30 days using AI-assisted development. The goal isn't just to ship—it's to document the actual workflow of building with an AI coding assistant.

This post covers Days 3-5, which I built in a single extended session with Claude Code. Total time: roughly 4 hours for three complete apps.

## The Workflow

My process with Claude Code is conversational. I don't write detailed specs. I describe what I want, we discuss it, Claude builds it, I test it, we iterate.

Here's how it actually works:

**Me**: "what's day 3? let's build day 3"

**Claude**: *(reads the roadmap, proposes a design, starts building)*

**Me**: *(tests it)* "yes! could we make it as a browser extension too?"

**Claude**: *(builds the extension)*

**Me**: "nope, this is great. let's publish"

**Claude**: *(creates GitHub repo, pushes, deploys to GitHub Pages)*

That's the rhythm. I steer, Claude builds. My actual typing is mostly short confirmations and course corrections.

---

## Day 3: Text Cleaner

**Time**: ~45 minutes
**What I built**: Web tool + Chrome extension for cleaning garbage text

### The Problem

Copy text from a PDF or Word doc. Paste it somewhere. It's full of garbage—extra spaces, "smart quotes" that break code, line breaks in weird places, invisible Unicode characters.

### The Build

I asked Claude to build a text cleaning tool. It proposed:
- Multiple cleaning options (checkboxes)
- Live preview of cleaned text
- Copy to clipboard

Then I asked: "could we make it as a browser extension too?"

Claude built a Manifest V3 Chrome extension with:
- Context menu integration (right-click → Clean Text)
- Popup for quick access
- Content script for in-page cleaning

One interesting technical detail: Chrome's Manifest V3 service workers can't directly access the clipboard. Claude handled this by routing through the popup context where `document.execCommand('copy')` works.

### What I Actually Typed

```
"what's day 3? let's build day 3"
"yes! could we make it as a browser extension too?"
"nope, this is great. let's publish"
```

Three messages. Claude did the rest.

### Links

- **Try it**: https://ilmych.github.io/day-03-text-cleaner/
- **Code**: https://github.com/ilmych/day-03-text-cleaner

---

## Day 4: AI Flashcard Quiz

**Time**: ~1 hour
**What I built**: Paste text → get AI-generated flashcards → quiz yourself

### The Problem

Studying is easier when you quiz yourself. But making flashcards manually is tedious. What if AI could read your notes and generate the questions?

### The Build

This one uses Google's Gemini API. The user brings their own API key (BYOK pattern), so there's no backend needed.

The flow:
1. User pastes any text (notes, article, chapter)
2. Gemini extracts key concepts and generates Q&A pairs
3. User flips through flashcards with a 3D animation
4. Quiz mode tracks correct/incorrect

Claude suggested keyboard shortcuts for power users:
- Space: flip card
- →: mark correct
- ←: mark wrong
- S: shuffle
- R: restart

The 3D flip animation is pure CSS—`transform: rotateY(180deg)` with `transform-style: preserve-3d`. No JavaScript animation libraries.

### The Prompts

My prompts:

```
"what's day 4"
"yes, let's build it!"
"ooh i like it! great design job! i like this a lot. do we need to add anything else to this?"
"yes let's publish it"
```

The prompt Claude wrote for Gemini:

```
Extract 5-10 key facts, concepts, or important points from this text.
For each one, create a flashcard with:
- A clear, specific question
- A concise answer (1-2 sentences max)

Return as JSON array: [{"question": "...", "answer": "..."}, ...]
```

### The BYOK Pattern

I want to highlight the **Bring Your Own Key** approach:

- No backend infrastructure needed
- No API rate limits to manage
- No costs for me to scale
- User has full control
- Works offline after initial generation

The tradeoff is UX friction—asking users for an API key. But for a free tool, it's worth it. Gemini's free tier is generous, and getting a key takes 30 seconds.

### Links

- **Try it**: https://ilmych.github.io/day-04-flashcard-quiz/
- **Get API key**: https://aistudio.google.com/apikey
- **Code**: https://github.com/ilmych/day-04-flashcard-quiz

---

## Day 5: Deadline Dice

**Time**: ~2 hours
**What I built**: Shake your phone to get a random deadline (iOS app)

### The Problem

"I'll do it sometime this week" is a lie. Vague deadlines don't get done.

But picking a specific time feels arbitrary. Why Wednesday? Why 2pm?

Solution: let the dice decide. Now you have a deadline AND an excuse.

### The Build

This was the most complex build of the three—first mobile app of the challenge.

Stack:
- **Capacitor**: Wraps web app as native iOS
- **DeviceMotion API**: Detects phone shaking
- **Capacitor Haptics**: Native vibration feedback
- **Single HTML file**: All UI and logic

The core shake detection:

```javascript
window.addEventListener('devicemotion', (e) => {
    const acc = e.accelerationIncludingGravity;
    const total = Math.abs(acc.x) + Math.abs(acc.y) + Math.abs(acc.z);
    if (total > 25) {
        rollDice();
    }
});
```

### The Bugs

Mobile development has platform-specific quirks. Here's what we hit:

**Bug 1: Touch not working on iOS**

The dice area wasn't responding to taps. Turned out iOS needs explicit CSS:
```css
-webkit-user-select: none;
touch-action: manipulation;
```
Plus event listeners for both `click` AND `touchend`.

**Bug 2: Shake not triggering**

iOS 13+ requires explicit permission for motion sensors—and you have to request it during a user interaction (tap/click), not on page load. Claude fixed this by requesting permission on first tap.

**Bug 3: "Today" giving tomorrow**

If you selected "Today" at 9pm, it would sometimes give you a deadline for tomorrow morning. Fixed the logic to respect midnight as the boundary. "Today" means today, period.

**Bug 4: Week/month boundaries**

"This Week" was calculating based on 7 days from now, not "until Sunday." "This Month" was similar. Fixed to use actual calendar boundaries—week ends at midnight Sunday, month ends at midnight on the last day.

### The Prompts

```
"what's day 5"
"yes let's build it! i love this one. can we do a mobile app where shaking your phone rolls the dice?"
"tapping doesn't work"
"cool, the shake doesn't work though"
"ok so i chose today, but it gives me deadlines that are tomorrow. we need a midnight cutoff instead of 24h"
"week should also be until midnight sunday, and month should be until midnight last day of current month"
"awesome! i think we can publish this"
```

Notice the pattern: I test, report issues in plain language, Claude fixes them. No stack traces, no detailed debugging—just "tapping doesn't work" and Claude figures out the iOS-specific fixes.

### What I Learned About Capacitor

Capacitor is underrated for simple mobile apps. If your app is mostly UI with a few native features (haptics, sensors, camera), you don't need React Native or Flutter.

Write it as a web app. Add Capacitor. Run `npx cap sync ios`. Open in Xcode. Ship.

The learning curve is way lower than native development, and for most apps, the performance is indistinguishable.

### Links

- **Code**: https://github.com/ilmych/day-05-deadline-dice

---

## The Numbers

| Day | App | Time | Files | Lines of Code |
|-----|-----|------|-------|---------------|
| 3 | Text Cleaner | 45 min | 6 | ~500 |
| 4 | Flashcard Quiz | 1 hr | 1 | ~600 |
| 5 | Deadline Dice | 2 hrs | 25 | ~700 (web) |

**Total**: ~4 hours for 3 complete, deployed apps

### My Contribution vs Claude's

What I did:
- Decided what to build
- Approved designs
- Tested on device
- Reported bugs in plain language
- Made product decisions ("add keyboard shortcuts", "use BYOK pattern")

What Claude did:
- Proposed designs and architectures
- Wrote all the code
- Fixed bugs based on my reports
- Created GitHub repos
- Wrote commit messages
- Deployed to GitHub Pages
- Generated app icons (via Gemini API)

Rough estimate: I typed maybe 50 lines of text total. Claude wrote several thousand lines of code.

---

## Patterns I'm Noticing

### 1. Single-File Apps Are Underrated

All three apps are essentially single HTML files (plus extension files for Day 3). No build step, no npm, no bundler. You can open them directly in a browser.

This simplicity is a feature. Less tooling = less that can break.

### 2. BYOK (Bring Your Own Key) Works

For AI-powered tools, asking users to provide their own API key eliminates the need for a backend. It shifts the cost to users (free tier is usually fine) and gives them full control.

### 3. Platform Quirks Are Real

iOS has specific requirements for touch handling, motion permissions, and Safari behavior. Claude knew about these and applied the fixes, but I had to test on device to discover them.

### 4. Plain Language Bug Reports Work

I don't send stack traces or error messages. "Tapping doesn't work" is enough. Claude figures out what's likely wrong and fixes it.

### 5. The 80/20 of Mobile Development

Most "mobile apps" are really just web apps that need a few native features. Capacitor handles this perfectly. Save the native development for apps that truly need it.

---

## What's Next

Days 6-7 are already partially built:
- **Day 6**: Sleep Cutoff Calculator (already deployed as iOS app)
- **Day 7**: Weekly Reflection Journal

We're 5/30 complete. 17% of the way there.

Follow along: **#30DaysOfVibeCode**

---

*All code is open source. Links to each repo are in the individual project sections above.*
