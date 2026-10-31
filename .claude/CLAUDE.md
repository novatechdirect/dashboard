# NovaTech dashboard

Plain HTML, CSS and JavaScript with no build step. It is served by GitHub Pages at
https://dash.novatechgaming.com from the root of `main`. The backend is Supabase
(project "LabelMaker", ref `vajrnbniwkvnkkkigeit`), reached through the Supabase connector.

This repo is public, including this file. Never put secrets, customer data or internal
notes in it.

| File | What it is |
| --- | --- |
| `index.html` | Staff dashboard. About 1.5 MB, so search it rather than reading it whole. |
| `portal.html` | Customer order portal, with its own login. |
| `track.html` | Shipment tracking page. |
| `dashboard/index.html` | Redirect to `/`, for old links. |
| `.github/workflows/keepalive.yml` | Pings Supabase every 3 days so the project doesn't pause. |

## Working rules

The owner is not a developer. Explain changes in plain language.

### Code

- Run `git pull` at the start of every session. Work may have come from the other computer.
- Anything pushed to `main` goes live within minutes, for staff and customers.
- Before pushing to `main`: test the change on this computer, show the owner the result, and
  wait for their approval.
- To move unfinished work to the other computer, push it to a `wip/<topic>` branch. Only `main`
  is served, so this doesn't affect the live site. The branch is still publicly readable.
- Editing `.github/workflows/` needs an extra permission: `gh auth refresh -s workflow`.

### Database

- There is no staging database. Supabase is live production data.
- Local test pages connect to that same live database, so anything saved while testing is real.
  Test read-only where possible, and tell the owner before a test would write data.
- Reading data and table structure is fine without asking, except `app_secrets`. Don't read or
  print its values unless the owner asks for that specifically.
- Before any write: show the owner the SQL, explain in plain language what it does and what
  data it touches, and wait for their approval.
- The connector has full admin rights and bypasses row-level security. Be careful, and check
  the policies when adding tables or changing access.
- Run schema changes with the `apply_migration` tool, so they are recorded in Supabase's
  migration history. That history starts empty because earlier scripts were run by hand.
- Don't commit SQL files. This repo is public and is served as a website, so they could be
  read there.

### Data that must stay intact

- `serial_burn` is append-only and never cleaned up. It stops deleted orders from reusing serials.
- `staff` is the source of truth for permissions. `tc_employees` is a view over it.

## Setting up another computer

1. Install Git and GitHub CLI (`gh`).
2. Sign in with `gh auth login --hostname github.com --git-protocol https --web`, then run
   `gh auth setup-git`.
3. Run `git clone https://github.com/novatechdirect/dashboard.git`.
4. Set `git config --global user.name` and `user.email` to match the existing commits.
5. Check that the Supabase connector is connected. It comes from the Claude account, not the
   computer.

Chat history and Claude's memory stay on each computer. Anything both computers need to know
belongs in this file.
