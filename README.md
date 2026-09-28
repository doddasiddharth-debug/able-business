# ABLE Business · Free courses

Free, self-paced courses from ABLE Business, the financial-literacy and
entrepreneurship branch of [ABLE Initiatives](https://ableinitiatives.com), a
student-run 501(c)(3) nonprofit. Live at **https://business.ableinitiatives.com**.

Static site, no build step, no accounts: serve the folder with any static
server (`python3 -m http.server`) or let GitHub Pages deploy it on every push to
`main` (`.github/workflows/static.yml`). It is the sibling of the SAT course at
prep.ableinitiatives.com and uses the same app shell.

## Courses

| Course | Views | Progress key |
|---|---|---|
| **Money &amp; Business Foundations**: budgeting; paychecks and taxes; saving and investing; credit and debt; how a business makes money; starting something, and careers | `#mbf`, `#mbf-lesson-1`…`6`, `#mbf-certificate` | `able.business.course.v1` |
| **Financial Literacy: Money in Real Life**: banking basics; smart spending; scams and identity theft; insurance; paying for college; your first car and apartment | `#fl`, `#fl-lesson-1`…`6`, `#fl-certificate` | `able.business.fl.v1` |

Each course has a dashboard (progress ring, continue button, stats, lessons,
its calculators), six lessons (goals, worked example, common mistake, key
idea, key terms, "try it yourself", a five-question quiz where four right
completes the lesson) and its own certificate. Shared views: **All courses**
(`#home`, the catalog and the default), **Calculators** (`#tools`, grouped by
course, each linkable as `#tool-…`) and **Glossary** (`#glossary`, every
course's key terms A to Z, built at runtime from the lessons).

Old links from before there were several courses still work: `#dashboard`
and `#lessons` open the Foundations dashboard, `#lesson-N` its lesson N, and
`#certificate` its certificate (`alias()` in `app.js`).

## Adding a course

1. Write six `<article class="lesson">` blocks like the existing ones, with
   ids and quiz radio names prefixed so nothing collides (`fl-lesson-3-title`,
   `name="fl3-2"`), and no `id` on the article itself.
2. Add its views to `index.html` with `data-course="<id>"`: a dashboard
   (`id="<id>"`), one section per lesson (`id="<id>-lesson-N"`) and a
   certificate (`id="<id>-certificate"`, `data-course-cert="<id>"`); a
   sidebar group (`data-course-nav`); and a `.course-tile` on `#home`.
   Copy the Financial Literacy markup; it is the template.
3. Add an entry to `COURSES` in `app.js` (storage key, title, the two topic
   lines printed on the certificate, file-name slug).
4. New calculators go in `TOOLS` in `app.js` and in the calculators view.

## Files

```
index.html               every view of every course (edit lessons here)
assets/css/business.css  app shell, course components, catalog
assets/js/app.js         router, per-course progress, quizzes, calculators,
                         glossary, certificates
assets/images/           ABLE Business mark, ABLE mark (certificate seal), favicon
CNAME                    business.ableinitiatives.com
```

## How it works

- **Without JS** the page is every course top to bottom; calculators hide
  and quiz explanations show. **With JS**, one view shows at a time, chosen
  by the URL hash, and the sidebar lists only the open course's lessons.
- **Progress** is per course in `localStorage`: `passed`, the `best` score
  per lesson, the certificate `name`, and `completedAt` (the day the last
  lesson was first passed, printed on the certificate). Nothing is sent
  anywhere, so progress is per browser. "Reset my progress" clears every
  course. The first course's old page on ableinitiatives.com forwards here
  with `?progress=`, which is merged into Foundations.
- **Calculators** are `.lesson-tool[data-tool]` blocks: inputs `[data-in]`,
  outputs `[data-out]`. Each appears in its lesson and on the calculators
  page, and its default inputs reproduce that lesson's worked example, so
  keep the two copies' defaults in step. Loan payments are
  P·r/(1−(1+r)^−n) rounded to the cent, as in the lessons; the card-payoff
  tool rounds interest each month because Foundations lesson 4's table does.
- **Certificates** have no signature and no number on purpose: completion is
  recorded only in the student's browser, so they can't be verified and
  shouldn't look as if they can.

## Content rules

Every number is worked out, not estimated. No year-specific figures that
change annually (tax brackets, retirement-account limits, wage caps). Returns
are described as historical, never promised. No brands, products or specific
investments, and no views attributed to real people; example students are
made up. It is general education, not financial, tax or legal advice, and the
footer says so. **An ABLE Business officer should read any new or changed
lesson before it goes live.**

## Visitor analytics

Anonymous visitor counts come from [GoatCounter](https://www.goatcounter.com)
(`assets/js/analytics.js`): no cookies, no personal data, nothing that
identifies a visitor, so no cookie banner is needed. One GoatCounter site
(code `siddo`, dashboard at https://siddo.goatcounter.com)
covers every ABLE site: ableinitiatives.com, prep. and business.ableinitiatives.com,
and Strands of Life. Each path is prefixed with its host to keep them apart.
The same `analytics.js` is copied into each repo; keep the copies in step.

Besides page views it records, as events: clicks on email links
(`email/…`) and on links to other sites (`outbound/…`), and in the course apps
`window.ableTrack(...)` calls (quizzes passed or failed, calculators used,
courses completed, certificates made, downloaded or printed; SAT sessions
finished). Visits from localhost are not counted.
