# Break Even

A personal budget and expense tracker in a single web page. It tracks accounts, transactions, recurring bills, budgets and categories, and shows how much you can still spend before your next payday.

Live site: https://breh101.github.io/break-even/

## Features

- **Home**: account balances, this month's income and spending, and "Available before payday", which is the amount you can still spend.
- **Accounts and transactions**: add, edit and delete accounts and transactions. Transfers move money between accounts. Card spending is tracked separately from checking.
- **Fund (optional)**: turn on the monthly fund with the checkbox in the Monthly fund section on the home screen. When it's on, you can set aside an amount from each paycheck, and that amount is subtracted from upcoming paychecks in the projection. When it's off, the fund is hidden everywhere.
- **Bills**: recurring bills with due dates, categories and month-by-month navigation. Mark a bill as paid by recording its payment.
- **Budgets**: monthly spending limits per category. Changes can apply to this month only or to every month.
- **Categories and sections**: group categories into sections, and rename or remove them.
- **Income breakdown**: this month's income, grouped by category.
- **Backup**: copy a backup of your data, or restore one from pasted text.
- **Cloud sync (optional)**: sign in on the Backup page to sync data across devices through Supabase.
- **App lock (optional)**: set a PIN that locks the app after you've been away for more than one minute. On the GitHub Pages version, Face ID can unlock it on supported devices.
- **Navigation**: tap the tabs, or swipe left and right between screens. Each screen remembers its scroll position.

## Using it on an iPhone

1. Open the live site in Safari.
2. Tap Share, then **Add to Home Screen**.
3. Open the app from the Home Screen.

The app updates itself when you open it. The update check only exists in copies added after this feature was released, so if the app doesn't update, delete the Home Screen icon and add it again once.

## Where your data lives

- Data is stored in the browser's local storage on each device.
- If you sign in to cloud sync, your data is also saved to your Supabase account, and each device pulls from there.
- Use the Backup page to keep your own copy of your data.

Clearing the browser's site data erases the local copy.

## Project files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: markup, styles and script in one file. |
| `apple-touch-icon.png` | Icon used for the Home Screen and the title bar. |
| `version.json` | Build label that the running app compares against to detect updates. |

## Versioning and deploys

The build label is set in two places and must match:

- `APP_BUILD` in `index.html`
- `build` in `version.json`

Use a new label, such as `2026-11-24a`, for each release. The live site is served from `main`, so commit and push there:

```
git push origin HEAD:main
```

## Cloud sync setup

Sync uses Supabase with one table, `budget_data`, which holds one JSON row per user:

- `user_id uuid` primary key, references `auth.users`
- `data jsonb`
- `updated_at timestamptz`

Row Level Security must stay on. Policies allow each signed-in user to select, insert and update only their own row (`auth.uid() = user_id`). The app uses only the publishable key. Never put a service key in this repository.

## Development notes

- The app is plain HTML, CSS and JavaScript, with no build step.
- To check syntax, extract the largest `<script>` block and run `node --check` on it.
- Test in a desktop browser at a phone-sized viewport before pushing.
