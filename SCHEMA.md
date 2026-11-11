# Edition data format

Each edition is one JSON file, `editions/YYYY-MM-DD.json` (UTF-8). Turn it into the website with:

    python3 render.py editions/YYYY-MM-DD.json

That checks the edition, writes `editions/YYYY-MM-DD.html`, and rebuilds `site/` (the folder Cloudflare
serves: `index.html` = newest edition, `editions/`, `archive.html`, icons). The renderer handles all
layout, fonts, colours and images, so the JSON holds content only. Never put HTML tags in strings.
Plain text only; use real characters (é, —, « », ’) rather than entities.

The edition is bilingual. Stories from French-language sources are written in French; stories from English-language
sources are written in English and marked `"lang": "en"`. The interface (section names, labels) stays French.
Quotes stay in the language they were said in.

```json
{
  "date": "2026-11-10",
  "date_long": "mardi 10 novembre 2026",
  "issue": 1,
  "generated_at": "06 h 15",

  "quote":  { "text": "…", "speaker": "Prénom Nom", "context": "ministre de l'Immigration, en point de presse", "url": "https://…" },
  "number": { "value": "380 000", "text": "Ce que le chiffre mesure, en une phrase.", "url": "https://…" },

  "agenda": [                             // optional: 3–6 upcoming dates worth watching, soonest first
    { "date": "12 nov.", "text": "Ce qui est attendu ce jour-là", "url": "https://…" }
  ],

  "stories": [
    {
      "id": "plan-niveaux-2027",          // unique kebab-case (a-z, 0-9, hyphens); the article page anchor
      "section": "immca",                 // colleges | etudiants | superieur | ecoles | immqc | immca | provinces | droit | chiffres
      "lead": true,                       // exactly ONE story is the lead (À la une)
      "featured": true,                   // exactly 3 stories (not the lead, 3 different sections) in the row under the hero
      "brief": "Titre court d'une ligne pour « L'essentiel du jour »",   // 5–8 stories have one
      "kicker": "Ottawa",                 // short grey word(s) above the headline on the article page (place, body, topic)
      "suptitle": "Plan des niveaux 2027-2029",  // short italic line above the headline on cards
      "title": "Titre de l'article",
      "standfirst": "Résumé d'une ou deux phrases. Sert sur les cartes et comme légende de photo.",
      "body": ["Paragraphe 1", "Paragraphe 2", "…"],   // 3–6 paragraphs written from the source(s)
      "why": "Une ou deux phrases : pourquoi c'est important pour la lectrice.",
      "facts": ["Fait 1", "Fait 2"],      // 2–5 points
      "opinion": false,                   // true for columns, op-eds, interviews, political statements
      "lang": "en",                       // optional: "en" for a story written in English (from an English-language source); omit for French
      "source":  { "name": "Radio-Canada", "url": "https://…", "date": "9 novembre 2026" },
      "extra_sources": [ { "name": "La Presse", "url": "https://…" } ],
      "image": { "url": "https://…jpg", "credit": "RADIO-CANADA" }   // optional; if missing the renderer uses the source page's og:image, else a typographic panel; set to null when the only image is a logo
    }
  ],

  "dossiers": [
    {
      "id": "plafond-etudiants-etrangers",
      "section": "etudiants",
      "title": "Titre du dossier",
      "status": "new",                    // "new" (development in the last ~48 h) or "nochange"
      "update": "Ce qui est nouveau aujourd'hui (ou un rappel d'une ligne si rien n'a changé).",
      "background": ["Paragraphe", "…"],  // what the story is, including anything that could not be verified
      "timeline": [ { "date": "22 janv. 2024", "text": "Ce qui s'est passé", "url": "https://…" } ],   // oldest first
      "image": { "url": "https://…", "credit": "…" }   // optional
    }
  ],

  "sources": [ { "title": "Titre de l'article", "outlet": "Média", "url": "https://…" } ]
}
```

Sections (key: name):

| key | name | what goes in it |
|---|---|---|
| `colleges` | Collèges privés | Private colleges in Quebec, subsidised or not: permits, closures, sanctions, the Loi sur l'enseignement privé, associations, named colleges |
| `etudiants` | Étudiants étrangers | Study permits and caps (IRCC), CAQ, per-institution quotas, PGWP, effects on institutions |
| `superieur` | Cégeps et universités | Quebec higher education: the ministry, budgets, cégeps, universities, language rules |
| `ecoles` | Écoles privées | Private primary and secondary schools: public funding, permits, laïcité |
| `immqc` | Immigration Québec | MIFI, thresholds, PEQ/PSTQ, temporary workers, francisation, asylum seekers in Quebec |
| `immca` | Immigration Canada | IRCC, levels plan, asylum and border bills, Express Entry, citizenship |
| `provinces` | Ailleurs au Canada | Private career colleges and immigration moves in the other provinces |
| `droit` | Lois et tribunaux | Bills, regulations, Gazette officielle, judgments, watchdog reports |
| `chiffres` | Chiffres et études | Statistics Canada, ISQ, think tanks, polls |

Rules (render.py refuses to build the site when the first four are broken):
- Exactly one `lead`, exactly three `featured` (not the lead), 5–8 stories with a `brief`.
- Every story has a real `source.url` that was actually read. Never invent a link, a fact, a figure or a quote.
- Ids are unique kebab-case. Sections are the nine keys above.
- No HTML in any string.
- A section with no fresh news is simply left out that day (the page hides empty sections).
- The dossiers listed in state.md are always present, with their full timeline carried forward.
- Dates are written in French: "9 novembre 2026" in `source.date`, "9 nov." or "9 nov. 2026" in timelines.
