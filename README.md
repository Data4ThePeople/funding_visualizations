# Who Do You Trust for the News?

An interactive, single-page timeline from **Data 4 The People** that traces how Americans lost a shared set of facts, from the penny press to AI-generated news, and shows the path we're building to earn that trust back.

It was built as the visual backbone of a short (about 7-minute) presentation on our vision, work and goals. It also works on its own as a shareable web page.

## Purpose

The page makes one argument, in order:

1. **The broadcast era had shared (if partial) truth.** Newspapers, radio and television were filtered by a few gatekeepers, but most people saw the same picture, and professional ethics codes and ad-funded newsrooms supported the work.
2. **The internet broke the model.** Blogs, then social media, then paid influencers let anyone reach an audience, while the money that paid for reporting drained away from newsrooms.
3. **AI raises the stakes.** Content can now be produced at scale with little human oversight, and chatbots are becoming a new middleman between people and the news.
4. **There is a different path.** Data 4 The People pairs AI-assisted research tools with a mentor–mentee peer-review network, so curious creators can produce data journalism that is fact-checked, transparent, and open for anyone to check.

It ends with two links: our current site and our labs site, where we demo what's next.

## How it works

The timeline is a single path. The current step always sits in the center of the screen, and the camera pans to each new step.

- **The line's color shows credibility.** It runs from muted green in the broadcast era, through amber, to red in the AI era, then turns bright green on our corrective path. The credibility color and the side meter are an **editorial illustration of the argument, not a measured index**, and the page says so.
- **Gray paths show roads not taken.** One shows a world without the internet; the other shows continuing on today's course.
- **Direction carries meaning.** The historical path runs down, the internet era runs down and to the right, and our path turns *up and to the right* from "Where we are now."

### Steps

| # | Step | Era |
|---|------|-----|
| 0 | Title: *Who do you trust for the news?* | |
| 1 | The printed page | 1830s onward |
| 2 | News at the speed of sound (radio) | 1920 onward |
| 3 | Television took over | 1950s–1980s |
| 4 | The internet changes everything | 1990s |
| 5 | Anyone can publish anything (blogs) | Late 1990s–2000s |
| 6 | Social media boom | 2004 onward |
| 7 | Reach becomes the business model (influencers) | 2008 onward |
| 8 | The newsroom gets squeezed | 2005–today |
| 9 | Is anyone even there? (AI) | 2023 onward |
| 10 | Where we are now | Today |
| 11 | Forward, not back | Data 4 The People |
| 12 | AI-assisted research | The tools |
| 13 | Mentors check the work | The standard |
| 14 | A network of curious creators | The community |
| 15 | Logo and links to our sites | |

## Sources and transparency

Every factual claim is sourced, as you'd expect from a data journalism organization.

- **Each card ends with a Sources line** of clickable links, and the **Sources** button collects every source in one list.
- **Lines marked "Our read"** are Data 4 The People's interpretation, kept visually separate from sourced facts.
- **Main sources:** Gallup, Pew Research Center, the Reuters Institute *Digital News Report 2026*, Northwestern Medill's *State of Local News 2025*, NewsGuard, CERN, Nielsen via TVB, the Vosoughi, Roy & Aral study in *Science* (2018), and the FDR Presidential Library.
- **One source is our own:** the "Mentors check the work" card cites the Data 4 The People program plan, which is not yet published. Replace it with a public link once one exists.

Statistics reflect the sources as of September 2026. The Reuters Institute figures are global (48 markets), not U.S.-only; the cards say "worldwide" where that applies.

## Controls

| Action | Keys and gestures |
|--------|-------------------|
| Next step | → ↓ Space, Page Down, Enter, scroll down, swipe up, or **Next** |
| Previous step | ← ↑ Page Up, scroll up, swipe down, or **Back** |
| First or last step | Home or End |
| Jump to a past step | Click its circle |
| Open a specific step | Add `#` and the step number to the address, e.g. `index.html#9` |

Most presentation clickers send arrow or Page Down keys, so they work out of the box. Test yours before presenting.

### Motion

- **By default,** the camera pans between steps and the active circle pulses.
- **If a viewer's device has "reduce motion" turned on,** the pan is shorter and the pulse is slower.
- **To turn off all animation,** add `?motion=off` to the address, e.g. `index.html?motion=off`.

## Presenting

The timeline takes about 3.5–4 minutes at roughly 15 seconds per step. That leaves about 3 minutes of a 7-minute slot for live demos of the two sites linked on the final step. If you run long, the easiest cuts are to merge the blogs and social media steps, or the tools and mentors steps.

## Running and deploying

The whole visualization is one self-contained file, `index.html`. The code, styling and logo are all inside it, and there is no build step and no server code.

- **Locally:** open `index.html` in any modern browser.
- **GitHub Pages:**
  1. Put `index.html` at the top level of the repository (or in `/docs`).
  2. In the repository, go to **Settings → Pages**, choose the branch (and folder), and save.
  3. The page goes live at `https://<user-or-org>.github.io/<repo>/`, usually within a minute or two.

GitHub Pages is free for public repositories; private repositories need a paid plan. After an update, force-refresh (Ctrl+Shift+R, or Cmd+Shift+R on a Mac) if you don't see the change.

**External requests:** the page loads the fonts Newsreader and Public Sans from Google Fonts, with fallbacks if they're unavailable. Everything else is built into the file.

## Editing the content

All text lives in the `<script>` near the bottom of `index.html`.

- **`S`** is the source list. Each entry has a label `t` and a URL `u`; an empty URL shows the label without a link.
- **`N`** is the list of steps. Each step can have:
  - `era`, `title` and `body`: the card text
  - `take`: an "Our read" interpretation, shown separately from facts
  - `stat`: one figure, or a list of two, each with `num`, `lab` and a short `src`
  - `src`: the list of sources from `S` shown at the bottom of the card
  - `cred`: a 0–1 value that sets the line color and the meter position
  - `x`, `y`: the step's position on the map; `side` sets which side of the circle the card appears on
- **Sources panel:** it builds itself from `S`, so adding a source there adds it everywhere.

Keep facts in `body` and `stat`, each backed by an entry in `src`. Keep argument in `take`.

## Links

- Data 4 The People: https://www.data4thepeople.com/
- Labs (where we're headed): https://labs.data4thepeople.com/

## Credits

Created by Data 4 The People. Logo © Data 4 The People. Cited statistics belong to their respective sources.

<!-- Add a license here if you want others to reuse the code, e.g. MIT for code with all rights reserved for the logo. -->
