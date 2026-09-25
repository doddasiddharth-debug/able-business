# ABLE Business · Money & Business Foundations

A free, self-paced course from ABLE Business, the financial-literacy and
entrepreneurship branch of [ABLE Initiatives](https://ableinitiatives.com), a
student-run 501(c)(3) nonprofit. Live at **https://business.ableinitiatives.com**.

Static site, no build step, no accounts: serve the folder with any static
server (`python3 -m http.server`) or let GitHub Pages deploy it on every push to
`main` (`.github/workflows/static.yml`). It is the sibling of the SAT course at
prep.ableinitiatives.com and uses the same app shell.

## What's in it

| View | What's there |
|---|---|
| **Dashboard** (`#dashboard`) | Progress ring, "continue" button, lessons passed / quiz questions right / certificate status, every lesson with its status, links to the calculators |
| **Lessons 1–6** (`#lesson-1` … `#lesson-6`) | Budgeting; paychecks and taxes; saving and investing; credit and debt; how a business makes money; starting something, and careers in business. Each: goals, worked example, common mistake, key idea, key terms, "try it yourself", a five-question quiz (four right completes it) |
| **Calculators** (`#tools`, `#tool-budget` …) | 50/30/20 budget, paycheck, compound growth, credit card payoff, break-even |
| **Glossary** (`#glossary`) | Every lesson's key terms, A to Z, searchable, built at runtime from the lessons |
| **Certificate** (`#certificate`) | Unlocks when all six quizzes are passed; drawn on a canvas with the student's name; download as PNG or print |

## Files

```
index.html               every view, including all six lessons (edit lessons here)
assets/css/business.css  app shell + the course components
assets/js/app.js         router, progress, quizzes, calculators, glossary, certificate
assets/images/           ABLE Business mark, ABLE mark (certificate seal), favicon
CNAME                    business.ableinitiatives.com
```

## How it works

- **Without JS** the page is the whole course top to bottom; calculators hide
  and quiz explanations show. **With JS**, one view shows at a time, chosen by
  the URL hash, so every lesson and calculator has a shareable link.
- **Progress** lives in `localStorage` under `able.business.course.v1`:
  `passed`, `best` score per lesson, the certificate `name`, and `completedAt`
  (the day the last lesson was first passed, printed on the certificate).
  Nothing is sent anywhere, so progress is per browser.
- **A quiz question** is a `fieldset.quiz-q` with `data-answer="A"`–`"D"`.
  Keep five per lesson, or change `PASS` in `app.js`.
- **Calculators** are `.lesson-tool[data-tool]` blocks, defined in `TOOLS` in
  `app.js`: inputs are `[data-in]`, outputs `[data-out]`. Each appears twice,
  in its lesson and on the calculators page; its default inputs match that
  lesson's worked example, so keep the two copies' defaults in step. The
  payoff tool rounds interest to the cent each month because lesson 4's table
  does.
- **The certificate** has no signature and no number on purpose: completion
  is recorded only in the student's browser, so it can't be verified and
  shouldn't look as if it can. Changing a lesson title means changing the
  topic lines in `drawCert` too, and the list on the dashboard and in the
  sidebar (which uses short titles).

## Content rules

Every number is worked out, not estimated. No year-specific figures that
change annually (tax brackets, retirement-account limits, wage caps). Returns
are described as historical, never promised. No brands, products or specific
investments, and no views attributed to real people; example students are
made up. It is general education, not financial, tax or legal advice, and the
footer says so. **An ABLE Business officer should read any new or changed
lesson before it goes live.**
