# Feature Map

One entry per user-facing feature. Update when a feature is added, renamed, moved, or its selectors change. A stale map is worse than no map if it points at the wrong selector.

## <Feature name>

- **Function:** <what it does>
- **How to reach it:** <route / menu path / action>
- **Keyboard shortcuts:** <keys, or "none">
- **DOM elements / selectors:** <stable selectors, e.g. `[data-testid="checkout-submit"]`>
- **Interactions:** <steps to perform the main action>
- **Expected result:** <observable outcome, including error and empty states>

## Example — Login

- **Function:** Authenticate an existing user and start a session.
- **How to reach it:** `/login`, or "Sign in" in the header on every page.
- **Keyboard shortcuts:** `Enter` submits the focused form.
- **DOM elements / selectors:** `[data-testid="login-email"]`, `[data-testid="login-password"]`, `[data-testid="login-submit"]`.
- **Interactions:** Fill email, fill password, press `Enter` or click submit.
- **Expected result:** Redirect to `/dashboard` and header shows the user's name. Wrong credentials show an inline error under the password field; empty fields disable submit.
