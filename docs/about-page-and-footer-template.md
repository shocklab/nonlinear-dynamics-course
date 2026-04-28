# About page + footer copyright — implementation guide

Concrete instructions for adding a standardised About page and copyright
footer to one of Jonathan Shock's UCT course websites. Reference
implementation: <https://shocklab.github.io/nonlinear-dynamics-course/about/>
(MAM2046W). Source repo: <https://github.com/shocklab/nonlinear-dynamics-course>.

This guide assumes a Hugo site with broadly the same structure as
MAM2046W. If file paths differ in your site, find the equivalents by
searching for the existing footer / nav / page-template markup.

---

## Tokens to substitute

Replace these throughout before committing:

- `{{COURSE_CODE}}` — e.g. `MAM1043H`
- `{{COURSE_TITLE}}` — long descriptive name, e.g. `First Year Mathematics for Scientists` (confirm with Jonathan)
- `{{COURSE_SHORT_TITLE}}` — snappy version for nav, e.g. `First Year Maths`
- `{{COURSE_LEVEL}}` — `undergraduate` / `postgraduate`
- `{{COURSE_TAGLINE}}` — for footer, e.g. `Based on Strogatz's <em>Nonlinear Dynamics and Chaos</em>` (or remove if no textbook)
- `{{REPO_OWNER}}` — usually `shocklab`
- `{{REPO_NAME}}` — repo slug
- `{{SITE_BASE_URL}}` — full URL with trailing slash, e.g. `https://shocklab.github.io/mam1043h-course/`
- `{{YEAR}}` — copyright year, e.g. `2026`

You'll also write three short paragraphs (`{{COURSE_BLURB_PARA1/2/3}}`)
and one audience paragraph (`{{AUDIENCE_BLURB}}`) — see the MAM2046W
examples at the bottom of this file for tone and length.

---

## Step 1 — Download Jonathan's photo

```bash
mkdir -p static/images
curl -sL -o static/images/jonathan-shock.jpg \
  "https://shocklab.github.io/Generative-AI-in-research-course/2017-03-15_JonathanShock_700-2.jpg"
```

File should be ~215 KB / 700×467. **Don't preview images** — verify with
`file static/images/jonathan-shock.jpg`. Jonathan has a global preference
against inline image previews in chat.

---

## Step 2 — Create `content/about.md`

````markdown
---
title: "About"
weight: 100
math: false
---

## About the Convenor

<div class="bio-block">
<div class="bio-photo">
<img src="{{SITE_BASE_URL}}images/jonathan-shock.jpg" alt="Jonathan Shock">
</div>
<div class="bio-text">

I'm **Jonathan Shock**, an Associate Professor in the Department of Mathematics and Applied Mathematics at the University of Cape Town. My research moves between theoretical physics, complex systems, and the application of machine learning to problems across the sciences and humanities. I wear a number of hats at UCT:

- Associate Professor, Department of Mathematics and Applied Mathematics
- Lead of [Shocklab](https://shocklab.net/supervising/), a research group of students and postdocs working across a wide range of topics
- Interim Director of the [UCT AI Initiative](https://ai.uct.ac.za)
- Co-PI of the [African Hub on AI Safety, Peace and Security](https://ai.uct.ac.za/african-hub-ai-safety-peace-security)
- PI of the African Compute Initiative

### Get in touch

For corrections, questions, or to suggest improvements: [jon.shock@gmail.com](mailto:jon.shock@gmail.com). More about me at [shocklab.net](https://shocklab.net).

</div>
</div>

## About This Course

**{{COURSE_CODE}}: {{COURSE_TITLE}}** is a {{COURSE_LEVEL}} course at the University of Cape Town. {{COURSE_BLURB_PARA1}}

{{COURSE_BLURB_PARA2}}

{{COURSE_BLURB_PARA3}}

## Who This Is For

The materials are designed first for UCT students enrolled in {{COURSE_CODE}}, but they are openly available for self-learners anywhere in the world. {{AUDIENCE_BLURB}}

## Licence

These materials are released under a [Creative Commons Attribution 4.0 International licence (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You are free to share and adapt them for any purpose, including commercially, provided you give appropriate credit, link to the licence, and indicate if changes were made.

This release is authorised under clauses 8.2 and 9.2.1 of the **UCT Intellectual Property Policy (2011)**, which assigns course-material copyright to the academic author and explicitly permits Creative Commons distribution. UCT retains a perpetual royalty-free non-exclusive internal-use licence (clause 8.2).

## How to Cite

If you use, adapt, or reference these materials in your own teaching or writing, please cite them as:

> Shock, J. ({{YEAR}}). *{{COURSE_CODE}}: {{COURSE_TITLE}}* [Course materials]. University of Cape Town. {{SITE_BASE_URL}}

## Contributing & Reporting Errors

The source for everything you see here lives on GitHub at [{{REPO_OWNER}}/{{REPO_NAME}}](https://github.com/{{REPO_OWNER}}/{{REPO_NAME}}). If you spot an error — a broken link, a misattributed citation, a confusing explanation, a wrong sign in an equation — the most useful thing you can do is open an issue or a pull request.

You can also email [jon.shock@gmail.com](mailto:jon.shock@gmail.com) directly. Corrections from outside readers have already meaningfully improved several pages.

## A Note on AI Assistance

The web conversion of these materials — the parsing of the original source files, the layout, any animation export, the navigation, and many of the formatting refinements — was carried out with substantial assistance from AI tools, primarily **Claude** (Anthropic). The mathematical content itself comes from the original lecture notes; the presentation has been reviewed and audited by a human, but errors will still occasionally slip through. If you spot one, please get in touch.
````

**Important:** the `<img src>` uses the full `{{SITE_BASE_URL}}` not a
leading-slash path. Raw `<img>` tags inside markdown bypass Hugo's
`render-image` hook, so a leading-slash path would 404 on the deployed
site even though `hugo server` makes it look fine locally.

---

## Step 3 — Append CSS for the bio block

Append to `static/css/main.css`. The `.notebook-content` prefix matches
MAM2046W's content wrapper class — adapt to whatever class your site
uses (or drop the prefix for global scope).

```css
/* ── About page bio block ── */
.notebook-content .bio-block {
  display: flex;
  gap: 2rem;
  align-items: flex-start;
  flex-wrap: wrap;
  margin: 1rem 0 2rem;
}
.notebook-content .bio-photo { flex: 0 0 220px; }
.notebook-content .bio-photo img {
  width: 100%;
  height: auto;
  margin: 0;
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
}
.notebook-content .bio-text { flex: 1; min-width: 280px; }
.notebook-content .bio-text > p:first-child { margin-top: 0; }

@media (max-width: 600px) {
  .notebook-content .bio-photo {
    flex: 0 0 100%;
    max-width: 240px;
    margin: 0 auto;
  }
}
```

---

## Step 4 — Add About to the top nav

Edit `layouts/partials/nav.html`:

```html
<nav class="site-nav">
  <a href="{{ .Site.Home.Permalink }}" class="site-title">{{COURSE_CODE}} — {{COURSE_SHORT_TITLE}}</a>
  <div class="nav-links">
    <a href="{{ .Site.Home.Permalink }}">Contents</a>
    <a href="{{ "about/" | absURL }}">About</a>
  </div>
</nav>
```

---

## Step 5 — Exclude About from chapter prev/next

The About page has `weight: 100` as a sentinel above any chapter
weight. Filter it out in `layouts/_default/single.html`:

```html
{{ define "main" }}
{{ $currentWeight := .Weight }}
{{ $sortedPages := where .Site.RegularPages.ByWeight "Weight" "lt" 100 }}
{{ $prevPage := false }}
{{ $nextPage := false }}
{{ if lt $currentWeight 100 }}
  {{ range $idx, $page := $sortedPages }}
    {{ if eq $page.Weight $currentWeight }}
      {{ if gt $idx 0 }}{{ $prevPage = index $sortedPages (sub $idx 1) }}{{ end }}
      {{ if lt (add $idx 1) (len $sortedPages) }}{{ $nextPage = index $sortedPages (add $idx 1) }}{{ end }}
    {{ end }}
  {{ end }}
{{ end }}
<article class="notebook-page">
  <header class="page-header"><h1>{{ .Title }}</h1></header>

  <div class="page-nav">
    {{ with $prevPage }}<a class="prev-next" href="{{ .RelPermalink }}">← {{ .LinkTitle }}</a>{{ end }}
    {{ with $nextPage }}<a class="prev-next next" href="{{ .RelPermalink }}">{{ .LinkTitle }} →</a>{{ end }}
  </div>

  <div class="notebook-content">{{ .Content }}</div>

  <div class="page-nav bottom">
    {{ with $prevPage }}<a class="prev-next" href="{{ .RelPermalink }}">← {{ .LinkTitle }}</a>{{ end }}
    {{ with $nextPage }}<a class="prev-next next" href="{{ .RelPermalink }}">{{ .LinkTitle }} →</a>{{ end }}
  </div>
</article>
{{ end }}
```

The `where ... "Weight" "lt" 100` excludes About from prev/next chains.
The outer `if lt $currentWeight 100` means About itself shows no
prev/next (standalone page).

If your home page lists chapters by weight ranges (e.g.
`{{ if and (ge .Weight 1) (lt .Weight 50) }}`), the About page is
naturally excluded — no change needed.

---

## Step 6 — Footer with copyright + licence

Edit `layouts/_default/baseof.html`:

```html
<footer>
  <p>{{COURSE_CODE}} · University of Cape Town · {{COURSE_TAGLINE}}</p>
  <p>&copy; {{YEAR}} Jonathan Shock. Licensed under <a href="https://creativecommons.org/licenses/by/4.0/">CC&nbsp;BY&nbsp;4.0</a>.</p>
</footer>
```

If the site uses Mathematica figures, also add (between the two lines):

```html
<p>Graphics generated with <a href="https://www.wolfram.com/mathematica/">Wolfram Mathematica</a></p>
```

If footer styling looks loose, add to the stylesheet:

```css
footer p { margin: 0.2rem 0; }
footer a { color: #888; text-decoration: none; }
footer a:hover { color: var(--accent); text-decoration: underline; }
```

---

## Step 7 — Verify and push

```bash
hugo
grep "Licensed under" public/index.html       # footer renders
grep "bio-block"      public/about/index.html # bio block renders
ls public/about/index.html                    # about page exists

git add -A
git commit -m "Add About page + copyright footer"
git push
```

After deploy (~30 s), check:

- `{{SITE_BASE_URL}}` — top nav has "About" link; chapter TOC does NOT list About
- `{{SITE_BASE_URL}}about/` — full About page renders with photo, all 7 sections, footer copyright; no prev/next nav
- `{{SITE_BASE_URL}}<some-chapter-page>/` — prev/next points to chapter neighbours (not About); footer copyright present

---

## Common pitfalls

- **Image 404s**: raw `<img>` inside markdown bypasses Hugo's
  `render-image` hook. Use full `{{SITE_BASE_URL}}images/...`, never
  `/images/...`.
- **Goldmark mangles inline JS**: not relevant for this About page,
  but if you ever need `<script>` inside markdown, put it in
  `static/js/widgets/foo.js` and load via `<script src="...">`.
- **Closing the bio-block divs**: the `</div></div>` goes AFTER the
  `### Get in touch` paragraph but BEFORE `## About This Course`.
  Don't close them earlier or the next heading will nest inside the
  flex container.
- **Citation year**: update `{{YEAR}}` in both the About page (How to
  Cite) and the footer when the course re-runs.

---

## Reference: MAM2046W's filled blurbs

For tone and length, here's how MAM2046W filled the blurb placeholders:

> **PARA1**: It builds on the first-year course MAM1043H (where one-dimensional dynamical systems were introduced) and extends those ideas to two- and three-dimensional systems where genuinely new phenomena — limit cycles, bifurcations, and chaos — emerge.
>
> **PARA2**: The course follows the structure of Steven Strogatz's *Nonlinear Dynamics and Chaos*, covering phase portraits, fixed points and linearisation, conservative and reversible systems, index theory, limit cycles, the Poincaré–Bendixson theorem, Liénard systems, relaxation and weakly nonlinear oscillators, saddle-node, transcritical, pitchfork and Hopf bifurcations, oscillating chemical reactions, global bifurcations, coupled oscillators, Poincaré maps, and the Lorenz equations as our gateway into chaos.
>
> **PARA3**: These notes have evolved through several years of teaching the course. The original lecture material was written in Wolfram Mathematica and has been converted to this web format so that the figures, animations, and equations can be browsed without needing Mathematica installed.
>
> **AUDIENCE_BLURB**: The mathematical prerequisites are modest — calculus, linear algebra, and a first course in differential equations are sufficient. If you have worked through Strogatz's textbook (or are working through it now), you should find these notes a useful complement, with worked examples, phase portraits, and animated bifurcations.

For MAM1043H you'll want to write equivalent paragraphs for the
appropriate level (first-year, prerequisite-light, presumably covering
topics like limits, derivatives, integrals, ODEs in one variable, or
whatever the actual syllabus is — confirm with Jonathan).

---

## File map (MAM2046W — for reference)

| File | Purpose |
|---|---|
| `content/about.md` | The About page content |
| `static/images/jonathan-shock.jpg` | Photo (downloaded once) |
| `static/css/main.css` | Bio-block CSS appended near the bottom |
| `layouts/partials/nav.html` | Top nav with About link |
| `layouts/_default/single.html` | Page template with weight-filtered prev/next |
| `layouts/_default/baseof.html` | Sitewide footer with copyright |

If MAM1043H has a parallel layout, edit those same paths. If not,
search for existing `<footer>`, `<nav class="site-nav">`, or
`{{ .Content }}` markup to find equivalents.
