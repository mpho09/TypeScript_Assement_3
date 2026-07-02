# Decision Log

At least 8 real entries. Generic entries score nothing. Use this shape:

## Decision: [what you decided]
- Context: Plain CSS with a small token set, no Tailwind or component library
- Options I considered: Plain CSS in a single styles.css with CSS custom properties as design tokens.
Tailwind,fast to write, huge utility surface.
- What I chose and why: I chose Plain CSS with named tokens (--paper, --ink, --stamp, --rule) lets the design vocabulary come from the subject matter (paper, ink, rubber stamps, ruled lines) instead of from Tailwind's
- What I gave up: : Speed on future features adding a new page means writing CSS by hand instead of composing utility classes.

---

## Decision: (1)
- Context:The catalog-card aesthetic wants a "call number" (LDG-001) on every card and detail slip.
- Options I considered: Add callNumber as a field on Item and populate it in the mock data.
erive it deterministically from id in a callNumberFor(id) utility.
Generate it randomly at render time.
- What I chose and why: The mock data represents "what a real API would return," and a real API almost certainly wouldn't return a display-only string that's a function of the id — it'd return the id and let the client format it
- What I gave up: The founder can't override the call number for a specific item (say, to match a legacy catalog).
