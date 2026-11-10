# Le Quotidien ÉPI

A private morning briefing, in French, on private education and immigration in Quebec and across
Canada (ÉPI = Éducation Privée, Immigration). One new edition a day, laid out like a newspaper
front page on a computer and like a news app on a phone.

It is the same machine as Le Quotidien IA, with other sections and a French interface.

## How it works

1. Each morning a scheduled Claude task follows `task/edition-quotidienne.md`: it reads `state.md`
   (what was already covered, the ongoing dossiers, the agenda), researches the news, and writes
   one file, `editions/YYYY-MM-DD.json` (content only, format in `SCHEMA.md`).
2. `python3 render.py editions/YYYY-MM-DD.json` checks the edition and rebuilds `site/`.
3. The task commits and pushes. Cloudflare serves the `site/` folder.

Nothing else is needed: no build step, no database, no API key. `render.py` uses only Python's
standard library (Pillow is used once, to draw the app icons, which are kept in `site/icons/`).

## Files

| | |
|---|---|
| `render.py` | Turns an edition into the website. Layout, colours, sections, all text of the interface. |
| `SCHEMA.md` | The edition format and the nine sections. |
| `task/edition-quotidienne.md` | The instructions the daily task follows. |
| `state.md` | Memory between runs. |
| `editions/` | One JSON per day. |
| `og-images.json` | Pictures already found on source pages. |
| `site/` | The published website. Written by `render.py`, never by hand. |
| `wrangler.jsonc` | Tells Cloudflare to serve `site/`. |

## Putting it online (Cloudflare)

1. Cloudflare dashboard > Workers & Pages > Create > Import a repository > choose this repository.
   No build command; the deploy command stays `npx wrangler deploy` (it reads `wrangler.jsonc`).
2. Settings > Domains & Routes > Add > Custom domain: the domain bought for her.
3. Recommended: Zero Trust > Access > add an application for that domain, policy "emails" with her
   address (and yours). Free for up to 50 people. She signs in with a code sent by email.

Keep it private. The pages are marked `noindex`, the photos are loaded from the news sites that
own them, and the layout is modelled on La Presse. That is fine for a family briefing behind a
login; it is not fine for a public or commercial site. Rebrand and drop the photos before opening
it up.

## Changing things

- Sections, their names and colours: the `SECTIONS` table at the top of `render.py`, then the list
  in `SCHEMA.md` and in the task file.
- The reader's interests, tone, sources: `task/edition-quotidienne.md`.
- City shown for the weather: `CITY` in `render.py`.
- Rebuild the whole site from the JSON files: run `render.py` on each edition, oldest first.
