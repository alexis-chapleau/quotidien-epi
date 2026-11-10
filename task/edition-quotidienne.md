# Daily task: produce and publish today's edition of Le Quotidien ÉPI

Produce today's edition of "Le Quotidien ÉPI", a private morning briefing in French on private
education and immigration in Quebec and across Canada, laid out like a newspaper front page
(desktop) and a news app (phone), then publish it by pushing this repository.

## The reader
One person: a senior Quebec public servant who works at the ministry on the committee that deals
with private colleges (permits, oversight, the Loi sur l'enseignement privé). She is an expert, so
never explain the basics; give her what changed, the exact figures, who said what, and what comes
next. She wants to open the page with her coffee and feel fully up to date. She reads French and
English. Anything touching private colleges in Quebec is the most valuable thing you can find,
even when it is small.

## Files (repository root)
- SCHEMA.md: the edition data format and the nine sections. READ IT FIRST and follow it exactly.
- render.py: checks an edition and turns it into the website in site/. Do not edit render.py and
  do not write any HTML yourself. Your job is the content.
- editions/YYYY-MM-DD.json: write today's edition here (today's date in America/Toronto).
- state.md: memory between runs. It holds the last issue number, the stories and links used in the
  last 7 editions (so nothing is repeated), the running timeline of each dossier, the agenda of
  upcoming dates, and notes for the next run. Read it before researching; update it at the end
  (drop editions older than 7 days).
- The previous edition's JSON in editions/ shows the tone and length to match. Carry every dossier
  forward with its whole timeline and add to it.
- site/: the published website (Cloudflare serves this folder on each push). Never edit it by hand.
- og-images.json: pictures already found for source pages. render.py maintains it.

## Sections (keys in brackets, in this order)
1. Collèges privés [colleges]: private colleges in Quebec, subsidised and non-subsidised: permits
   granted, renewed, modified, suspended or revoked; closures and bankruptcies; sanctions and
   investigations; the Loi sur l'enseignement privé and its regulations; the Commission consultative
   de l'enseignement privé; the Association des collèges privés du Québec (ACPQ) and the
   non-subsidised colleges' association; named colleges (LaSalle, Ellis, CDI, Herzing, Teccart,
   Universel, O'Sullivan, Matrix, M, Canada College, etc.); AEC and DEC programs; recruitment of
   international students by private colleges; student protection when a college closes.
2. Étudiants étrangers [etudiants]: federal study-permit caps and allocations, attestation letters,
   approval rates, post-graduation work permits, designated-institution compliance, fraud; Quebec's
   CAQ, the law capping international students and the per-institution quotas; effects on cégeps,
   universities and private colleges (enrolment, layoffs, program closures).
3. Cégeps et universités [superieur]: the Ministère de l'Enseignement supérieur and its minister,
   budgets and cuts, the Fédération des cégeps, the BCI, strikes, tuition, French-language rules in
   colleges and universities, capacity and infrastructure.
4. Écoles privées [ecoles]: private primary and secondary schools in Quebec: public funding debate,
   FEEP, permits, religious schools and laïcité, the three-tier school debate.
5. Immigration Québec [immqc]: MIFI and its minister, permanent thresholds and the multi-year plan,
   PEQ and PSTQ, Arrima draws, temporary foreign workers, francisation, family reunification,
   asylum seekers, Quebec–Ottawa disputes over powers and money, reactions from employers, regions,
   unions, universities and community groups.
6. Immigration Canada [immca]: IRCC and its minister, the Immigration Levels Plan, asylum and border
   legislation, the IRB backlog, Express Entry, temporary foreign worker program, citizenship,
   francophone immigration outside Quebec, processing times.
7. Ailleurs au Canada [provinces]: private career colleges and public-private partnerships in the
   other provinces (Ontario, British Columbia, Alberta…): closures, enforcement, new rules;
   college and university deficits tied to the cap; provincial immigration moves. Useful to her as
   comparisons.
8. Lois et tribunaux [droit]: bills at the National Assembly and in Parliament, draft and final
   regulations (Gazette officielle du Québec, Canada Gazette), decrees, court and tribunal
   decisions, lawsuits, reports from the Vérificateur général, the Auditor General, the Protecteur
   du citoyen, the Commissaire à la langue française, the Parliamentary Budget Officer.
9. Chiffres et études [chiffres]: Statistics Canada, Institut de la statistique du Québec, think
   tanks (Institut du Québec, IRIS, IEDM, C.D. Howe, Conference Board), bank economists, polls.

Put each story in exactly one section. A section with no fresh news is left out that day: do not
pad. A normal day has 10 to 16 stories; a quiet day (weekend, holidays) can have 6 to 9.

## Dossiers
The dossiers listed in state.md are always present. Add only new developments to a timeline
(oldest first, each with the link you read). Use status "new" when something happened in the last
~48 hours, otherwise "nochange" with a one-line recap in "update". Open a new dossier only when a
story has clearly become a long-running one (three or more developments over two weeks) and close
one, with a note in state.md, when it has been quiet for a month. Keep to four dossiers at most.

## Agenda
"agenda" lists 3 to 6 upcoming dated events worth watching (a bill's next stage, a court hearing, a
data release, a consultation closing, a budget update, an expected announcement), soonest first,
each with the link where you saw the date. Carry it in state.md, remove dates that have passed,
and add new ones as you find them.

## Research
- Start from state.md ("Notes for next run", follow-ups and the agenda), then search each section.
  Use WebSearch with French and English queries and include the current month and year. Focus on
  the last 24 to 72 hours; up to 7 days for slow sections and on Mondays.
- Use WebFetch to read every story you include: the body must come from the article you actually
  read. When a page cannot be fetched (paywall, refusal), find the same news at an outlet that can
  be read; never write a story from a search snippet alone, and never disguise the request to get
  past a refusal. Note unreadable sites in state.md so the next run does not retry.
- Go to the primary document whenever one exists (a release, a bill, a decree, a judgment, a
  statistical table) and link it in "extra_sources" next to the news article.
- Good sources: Radio-Canada, La Presse, Le Devoir, Le Soleil, Le Journal de Montréal and de
  Québec, TVA Nouvelles, La Presse canadienne, CBC, The Globe and Mail, Toronto Star, CTV, Global,
  Montreal Gazette; trade press (The PIE News, ICEF Monitor, University Affairs, CIC News);
  government pages (quebec.ca, canada.ca, assnat.qc.ca, Gazette officielle, Statistics Canada's
  The Daily); court decisions.
- Standing checks every day: news search for "collège privé" Québec, "collèges privés non
  subventionnés", "Loi sur l'enseignement privé", "étudiants étrangers", "permis d'études",
  "ministre de l'Enseignement supérieur", "ministre de l'Immigration" Québec, "seuils
  d'immigration", "PSTQ", "IRCC", "international students" Canada, "career college".
- Skip stories already in state.md unless there is a real new development.
- Never invent facts, quotes, numbers, dates, names, titles or links. If you are not sure of a
  detail, leave it out. Check every office-holder's name and title against a source from this
  month: a general election was held in Quebec in October 2026 and people changed posts.

## Writing
- French, the clear and sober voice of a good Quebec daily; no jargon, no hype, no filler. Short
  concrete headlines with a verb. Quebec usage (courriel, cégep, « guillemets français »).
- Quotes are given verbatim in the language they were said in; do not translate English quotes.
  Stories from English-language sources are written in French.
- Each story: a 1–2 sentence standfirst, 3–5 body paragraphs (what happened, the figures, the
  reactions, the context and what comes next), "why" (1–2 sentences on what it changes for someone
  who oversees private colleges or follows immigration policy), 2–4 "facts". Mark columns, op-eds,
  interviews and political statements "opinion": true.
- Exactly one "lead" (the most important story of the day for her), exactly 3 "featured" stories
  from 3 different sections, and 5–8 stories with a "brief" line.
- "quote" and "number": one real quote and one striking figure from today's sources, with links.
- Images: leave "image" out and render.py finds the picture on the source page; set "image": null
  when you know the only picture is a logo.
- issue = previous issue number in state.md + 1. generated_at = the current time in
  America/Toronto, written "06 h 15". date_long like "mercredi 11 novembre 2026" (no capital).

## Finish
1. Render: `python3 render.py editions/YYYY-MM-DD.json`. It refuses to build when the edition
   breaks a rule and lists what to fix; fix the JSON and run it again until it succeeds.
2. Update state.md.
3. Publish: `git add -A`, commit with the message "Édition YYYY-MM-DD (n° N)", then
   `git push origin main`. If the push fails, report the error; do not change remotes, credentials
   or git config, and never force-push.
4. Reply with a 3–5 line summary: the lead story, which dossiers moved, the number of stories and
   sections, anything that could not be read or verified, and whether the push worked.
