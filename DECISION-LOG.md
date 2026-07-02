# Decision Log

At least 8 real entries. Generic entries score nothing. Use this shape:

## Decision:(1) [what you decided]
- Context: Plain CSS with a small token set, no Tailwind or component library
- Options I considered: Plain CSS in a single styles.css with CSS custom properties as design tokens.
Tailwind,fast to write, huge utility surface.
- What I chose and why: I chose Plain CSS with named tokens (--paper, --ink, --stamp, --rule) lets the design vocabulary come from the subject matter (paper, ink, rubber stamps, ruled lines) instead of from Tailwind's
- What I gave up: : Speed on future features adding a new page means writing CSS by hand instead of composing utility classes.

---

## Decision: (2)
- Context:The catalog-card aesthetic wants a "call number" (LDG-001) on every card and detail slip.
- Options I considered: Add callNumber as a field on Item and populate it in the mock data.
erive it deterministically from id in a callNumberFor(id) utility.
Generate it randomly at render time.
- What I chose and why: The mock data represents "what a real API would return," and a real API almost certainly wouldn't return a display-only string that's a function of the id — it'd return the id and let the client format it
- What I gave up: The founder can't override the call number for a specific item (say, to match a legacy catalog).

## Decision: (3)
- Context:Home has a search bar, a filter drawer, and a grid of cards. Standard responsive question.
- Options I considered: Filters inline above the grid at every breakpoint.
Sticky filter sidebar on desktop, collapses to a top-of-page accordion on mobile.

- What I chose and why: I choose the sticky filter.The drawer sits to the right of the grid on desktop and stacks below on narrow screens. With only three filters the drawer is small enough to fit on-screen without hiding and the aesthetic depends on the drawer being visible. 

- What I gave up:  Vertical space on mobile. the filter drawer pushes results further down the page than a "Filters" button would.

## Decision: (4)
- Context: Booking requires auth. I could disable/hide the Book button when logged out or let unauth users click it and get bounced.
- Options I considered: Hide or disable the Book button when there's no user.
Let the Book button navigate to /book/:id regardless and guard the route with a RequireAuth wrapper that redirects to /login with a return path.

- What I chose and why: I chose the second option becouse Auth is a route-level concern, not a component-level one if I add three more auth-gated pages later, I want a single RequireAuth to handle them, not three buttons each doing their own check. 

- What I gave up: One extra page transition (detail → login → booking) instead of an inline modal. 

## Decision: (5)
- Context:itm_008 has status: "removed". A removed listing shouldn't be discoverable, but someone might have the URL bookmarked or shared.
- Options I considered:

- What I chose and why: Hide from catalog, but let the detail page render with a disabled Book button and a "Withdrawn" stamp.
Show removed items in the catalog with a "Withdrawn" stamp.

- What I gave up: I chose the last option becouseOwners withdraw listings for a reason clutter, sold, embarrassment, so they shouldn't keep showing up in browse. But someone following an old link deserves an honest answer, not a 404 that makes them think the app is broken. 

## Decision: (6)
- Context:paused items stay visible but are not bookable, even by direct URL
- Options I considered: Disable the Book button on the detail page and rely on that.
Also guard the /book/:id route so a paused item can't be booked even if a user edits the URL.
- What I chose and why: second Option becouse A disabled button is a UX affordance; it's not enforcement. Editing the URL to /book/itm_003 shouldn't produce a booking flow that then quietly succeeds — the Booking page itself checks status !== 'available' and shows a refusal screen. 

- What I gave up: Duplicated status-checking logic in two places.

## Decision: (7)
- Context: Treat every nullable field as a defined UI state, not an error
- Options I considered: Filter out incomplete items so the UI only ever sees "clean" data.
Centralize each field's "missing" representation in a format* helper (formatPrice(null) → "Ask owner", formatDistanceKm(null) → "Distance unknown", etc.).

- What I chose and why: the second option Centralizing means the string "Ask owner" is defined in exactly one place — if the founder later decides null price should say "Contact for quote" or should hide the item, I change one function, not eight components.

- What I gave up: A small amount of directness you have to open format.ts to know what a null renders as, instead of reading it inline. Worth it for the single source of truth.

## Decision: (8)
- Context: Load items via fetchItems() in each page instead of importing ITEMS directly
- Options I considered: Call fetchItems() in a useEffect on each page that needs data, render a loading state until it resolves.
Fetch once at the app root, pass items down through props or context.

- What I chose and why: I chose the first option
 Choosing the async path forced me to design a loading state for Home, ItemDetail, and Booking, which is the shape of the real problem.

- What I gave up: In a real app I'd cache with React Query or a context; noted as a follow-up. I also gave up compile-time knowledge of the item list

