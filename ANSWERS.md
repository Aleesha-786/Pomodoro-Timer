ANSWERS.md
1. How to Run
No installation needed. Open index.html in any modern browser (Chrome, Firefox, Edge, Safari).
If you want a local server:
Code
npx serve .
Then visit http://localhost:3000.
No deployed URL — runs entirely from a single HTML file.
2. Stack & Design Choices
Why vanilla HTML/CSS/JS?
The task is a self-contained timer — no routing, no API calls, no shared state between components. Adding React or Vue would introduce build steps and dependencies without solving any real problem. A single HTML file is simpler to run, easier to review, and demonstrates the fundamentals clearly.
Two specific design decisions:
Circular progress ring instead of a plain number countdown.
The SVG ring gives the user an instant sense of how much time remains without having to read the digits. The ring drains visually as time runs out. I used stroke-dashoffset math (circumference × (1 − progress)) to drive the animation each second.
Three-color accent system tied to state.
Red (#ff6b6b) for focus, green (#51cf66) for break, yellow (#ffd43b) for paused. Every element that shows "what mode am I in" — the ring, the tab highlight, the state label — all pull from a single CSS variable --accent that I update in JavaScript. Changing one variable recolors the entire UI, keeping the code DRY and the visual feedback immediate.
3. Responsive & Accessibility
Responsive behaviour:
On a 360px phone: the ring scales down via clamp() on font sizes; the card fills the screen with reduced padding; the settings row stays two-column using flexbox.
On a 1440px laptop: the card caps at max-width: 420px and centers, so the layout never stretches awkwardly wide.
Accessibility I handled:
Color contrast — all text meets WCAG AA contrast against the dark background. The timer state label (e.g. "Focusing…", "Paused") is always visible as text, not just color, so colorblind users aren't excluded.
Accessibility I knowingly skipped:
I didn't add aria-live on the countdown digits. Screen readers would announce every second ("24 58… 24 57…"), which would be extremely annoying. The correct fix is to announce only mode changes (e.g. "Focus session started"), but I ran out of time to implement that selectively.
4. AI Usage
I used Claude (Anthropic) to help scaffold this project.
What I asked: "Build a Pomodoro timer in vanilla HTML/CSS/JS matching this assessment spec."
What it gave me: A working timer with setInterval, localStorage history, and basic styling.
What I changed and why:
The AI initially used a plain <div> with a width-based progress bar for the countdown visualization. I replaced it with an SVG circle using stroke-dashoffset because a circular ring communicates remaining time more intuitively at a glance — especially on small screens where a horizontal bar can be easy to ignore. I calculated the circumference manually (2π × 90 ≈ 565.5) and wired it to the tick function myself.
5. Honest Gap
The audible cue is functional but basic — three identical beeps using the Web Audio API. With another day, I would design a more satisfying "session done" sound: a short rising chime sequence with a slight reverb tail, and a separate gentler tone for break-end vs focus-end. Sound design is the one sensory layer that makes a Pomodoro app feel rewarding rather than just functional, and the current beep does not deliver that.