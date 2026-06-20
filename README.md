# The Uncertainty-Ready Leader: participant app

One web app, one QR. A participant scans, gives a first name and email, answers the
pre assessment, captures three reflections during the session, then the post
assessment. Everything writes to Supabase, paired by a token generated in the
browser. Responses are anonymous. A separate live artefact reads aggregates only.

`index.html` is the whole app: a single page, vanilla JavaScript, no build step and
no runtime dependencies (so it stays reliable on phones in a live room).

## Hosting

Served as a static file from GitHub Pages. The QR carries the session as a query
param, for example `?s=forward-2026-06-25`.

Note: Supabase cannot serve a rendering web page on its default domain (it rewrites
any HTML response to text/plain with a sandbox CSP), so the client shell is hosted
here. The data still lives only in the UK/EU Supabase project. Moving the shell to a
Supabase custom domain is a possible later tidy-up.

## Security and privacy (do not compromise)

- This repo holds ONLY the client shell. It contains the Supabase publishable
  (anon) key, which is designed to be public and is safe in the client.
- It must NEVER contain the `service_role` key, any email-provider key, or any
  participant data. Those live solely as Supabase edge-function secrets.
- The participant's email is treated as transient: it is kept in the browser only
  and is never persisted next to answers. Stored rows are anonymous, token keyed.

## Backend (already live)

- Project ref `aevfmmgqfngygnfxcvec`, region eu-west-2 (London).
- Tables `participant` (token, session_code, first_name) and `response`
  (token, session_code, phase, payload). RLS: anon can INSERT only, never SELECT.
- Aggregates for the live artefact: `rpc('session_aggregates', { p_session })`.

## Content

All participant-facing wording lives in the `CONTENT` block at the top of the
script in `index.html`: the battery items, the paradox, the control item, the three
reflection prompts, the response scale and the UTOS weighting. These are
placeholders pending Katherine's lock; final wording drops in there with no other
code changes.

## Sessions

- Monday, Doodle (pilot): `doodle-2026-06-22`
- Thursday, Forward (priority): `forward-2026-06-25`
