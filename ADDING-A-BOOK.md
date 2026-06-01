# Adding a book to the Reading Library

This is the one-and-only file you need to add a new book. The whole site is a single
`index.html`; all the book data lives in a JavaScript list called `BOOKS` near the top
of that file (search for `var BOOKS = [`).

Everything else — the stats (books read, pages, reviews), the charts, the genre
breakdown, the search filters — **recalculates automatically** from that list. Covers
load **automatically** from the ISBN. So adding a book means inserting **one entry**.
Nothing else to edit.

---

## The easy way (what Jorg actually does)

Every couple of weeks, Jorg pastes Claude **one message**:

> Add this book: `<Goodreads link>` — my rating: `4/5`.
> Review: "…his own words…"

The Goodreads page gives the title, author, publication year, page count and ISBN.
Claude fills in the rest and pushes it live. That's it. No HTML, no images, no Excel.

---

## What Claude does (the procedure)

1. **Read the Goodreads page** for: title, author, publication `year`, `pages`,
   and the **ISBN-13** (13 digits, starts `978…`). The ISBN is what makes the cover
   appear, so always get it.
2. **Pick a genre** from the fixed list below (use the closest fit — don't invent new
   ones, or the genre chart gains a one-off slice).
3. **Write a short, neutral `about`** (2–4 sentences describing the book — not an
   opinion). Jorg's opinion goes in `review`.
4. **Use Jorg's rating and review** verbatim. Keep his wording; line breaks become `\n`.
5. **Insert the entry at the FRONT of the `BOOKS` list** (newest first), right after
   `var BOOKS = [`. Set `finishedDate` to the date Jorg finished it (default: today).
6. **Commit and push** to `main`. GitHub Pages redeploys in ~1 minute.
7. Confirm the cover shows. If Open Library has no cover for that ISBN (rare), download
   a cover image, save it as `covers/<isbn13>.jpg`, and add `"localCover": "<isbn13>.jpg"`
   to the entry as a fallback.

---

## The entry format

Add one object like this to the front of `BOOKS`:

```json
{
  "title": "Measure What Matters",
  "author": "John Doerr",
  "year": 2018,
  "pages": 320,
  "rating": 4,
  "genre": "Leadership & Management",
  "about": "A neutral 2–4 sentence summary of what the book is about.",
  "review": "Jorg's own review, in his words.\n\nLine breaks use \\n.",
  "isbn13": "9780525536222",
  "localCover": "9780525536222.jpg",
  "finishedDate": "2026-06-01"
}
```

### Field reference

| Field          | Required | Notes |
|----------------|----------|-------|
| `title`        | yes      | Book title. |
| `author`       | yes      | Author name. |
| `year`         | yes      | Publication year (number). |
| `pages`        | yes      | Page count (number) — feeds the "pages read" stat. |
| `rating`       | yes      | Jorg's rating, **1–5** (number). |
| `genre`        | yes      | **Must** be one of the genres listed below. |
| `about`        | yes      | Neutral synopsis. Not an opinion. |
| `review`       | yes      | Jorg's personal review. Use `\n` for line breaks. |
| `isbn13`       | yes      | 13-digit ISBN. Drives the auto-loading cover. |
| `localCover`   | optional | `"<isbn13>.jpg"` in `covers/`. Only needed if Open Library has no cover. |
| `finishedDate` | yes      | `"YYYY-MM-DD"`, when Jorg finished it. Newest books sort first. |

---

## The fixed genre list

Use one of these exactly (counts are illustrative):

- Leadership & Management
- Fiction
- Personal Development
- Business & Finance
- Biography & Memoir
- Science & Health
- Running & Sports
- Social & Cultural
- AI & Technology
- HR & People
- Other

---

## How covers work (why no image upload is needed)

Each card tries, in order:
1. A local file `covers/<isbn13>.jpg` **if** `localCover` is set.
2. Open Library: `https://covers.openlibrary.org/b/isbn/<isbn13>-L.jpg`.

Because step 2 only needs the ISBN, **you almost never have to touch the `covers/`
folder.** Just supply a correct `isbn13` and the cover appears. The `covers/` folder
exists only for the rare book Open Library doesn't have.

---

## Quick sanity check after pushing

- Open https://jvgaal.github.io/jorg-reading-library/ (wait ~1 min for redeploy).
- New book appears first in **My Books**, with a cover.
- The "Books Read" / "Pages Turned" / "Personal Reviews" stats each went up.
