# Wa$h Fitness Tracker

Single-file app: everything lives in `index.html` (HTML, CSS and one inline `<script>`). There is no build step and there are no dependencies.

## Deploying

- The app is hosted on GitHub Pages from the `main` branch (root) at https://dwa5hington.github.io/wash-fitness-tracker/, so a push to `main` is the deploy.
- It used to be hosted on Netlify (washfitnesstracker.netlify.app). That site stopped updating when the Netlify plan ran out of build credits, so don't rely on it.
- The phone's XP and diamonds live in the browser, keyed to the site's address. `HARDCODED_XP` and `HARDCODED_DIAMONDS` are the floor that survives a reset or an address change, so raise them when the owner reports higher numbers.
- When asked to "push to main", commit directly to `main` and push there. Don't use a side branch or open a PR unless asked.
- If the owner asks to review first, show the diff and wait for an explicit "yes" before committing or pushing.
- After a push, remind the owner to refresh the app on their phone (pull to refresh, or close and reopen).
- **Bump `<meta name="app-version">` in `index.html` on every push** (format `YYYY-MM-DD.N`, e.g. `2026-10-27.2`). The app checks this on open and reloads itself when the live version differs, so the Home Screen app doesn't get stuck on an old saved copy. Without a bump, the phone won't pick up the change.

## Adding a new day's plan

Use a kebab-case plan id, e.g. `tue-oct13-dynamic-upper-6040`, and update all of these places:

1. **Dropdown** (`<select id="plan-dropdown">`): add an `<option value="<id>">` at the top and move `selected` onto it.
2. **Plan panel**: add `<div class="plan-panel active" id="plan-<id>">` above the other panels, and remove `active` from the previous default panel.
3. **`currentPlan`**: set `var currentPlan='<id>';` so the new plan is the default.
4. **`PLAN_QUICK_EXERCISES`**: add `'<id>':[ ...exercise names in order... ]`.
5. **`PLAN_TYPES`**: add `'<id>':'SESSION TYPE LABEL'`, e.g. `'DYNAMIC UPPER FOCUS · 60/40'`.

## Logging a completed session

- Add an entry at the **top** of `var HARDCODED_SESSIONS=[`, following the format of the existing entries.
- `id`: `sess_<ms timestamp>`.
- `date`: formatted like `'Tue, Oct 13, 2026'`.
- `type`: matches the plan's `PLAN_TYPES` label.
- `lifts`: the standard empty lift map.
- `notes`: a written summary, e.g. `Exercise 135/155/175x8 Medium` or `Exercise 3 sets x20 sec Easy`. Mark new exercises `(NEW)` and substitutions `(SUB for X)`, and list exercises that weren't logged.
- Escape any apostrophes inside `notes`, since the string uses single quotes.

## Before pushing

- Extract the inline script and run `node --check` on it to catch syntax errors.
- Keep diffs minimal and don't reformat unrelated code. The file is large and minified-style.
