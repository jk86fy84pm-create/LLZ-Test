# AIR RANKING BW — LLZ Test

Responsive web portal for monthly 60-shot air rifle results in tenth-ring scoring.

## Current state

- Frontend is in `index.html`.
- `config.js` is a public configuration template; database values still need to be added.
- `supabase/schema.sql` creates the shared database table and basic row-level security.
- Until Supabase is configured, the portal displays clearly labelled demo data and does not save submissions.
- Submitted results start as `pending`; an administrator approves them in Supabase before they appear publicly.
- Never put a Supabase `service_role` key in frontend code or GitHub.

## 1. Configure Supabase

1. Create/open the LLZ Test project in Supabase.
2. Open **SQL Editor**, run all of `supabase/schema.sql`.
3. In **Project Settings → API** (or API Keys), copy the project URL and the anon/publishable key.
4. Replace the two placeholder strings in `config.js` with those values and commit the file.
   The anon/publishable key is intended to be public; database security is enforced by RLS.
5. In Supabase Database settings/publications, enable Realtime for `public.air_results` if available.
6. Use **Table Editor → air_results** to review submissions. Change `status` from `pending` to `approved` or `rejected`. Only approved results are visible on the portal.

## 2. Publish with GitHub Pages

1. Open repository **Settings → Pages**.
2. Under **Build and deployment → Source**, select **GitHub Actions**.
3. The workflow in `.github/workflows/pages.yml` deploys the static portal on pushes to `main`.
4. Check **Actions** and wait for the Pages workflow to finish successfully.
5. The resulting URL will be shown in **Settings → Pages**. For this repository it is normally `https://jk86fy84pm-create.github.io/LLZ-Test/`, but use the URL GitHub actually reports.

## Important limitations / production hardening

This initial version uses name + month as the duplicate guard. It is not proof of athlete identity, and an anonymous submission endpoint can be abused. Moderation prevents unapproved entries from appearing in the public ranking, but before a formal competition use, add athlete-specific authentication or invitation codes, submission rate limiting, and an authenticated admin workflow. Do not store sensitive athlete information.

## Scoring

The database stores integer tenths (e.g. 628.4 = `6284`) to avoid floating-point ranking errors. Results sort descending; equal scores share the same score but are listed alphabetically.
