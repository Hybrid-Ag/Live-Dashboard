# Editing the dashboard's written content

Everything on the dashboard that a **person writes** — meeting notes, the Great Southern
Biology write-up, the growth-routine targets, the stat chips in each meeting header —
lives in **`content.json`** in the root of this repo. The marketing calendar lives next to
it in **`mktcal.json`** (see `MKTCAL.md`).

Edit either file here, commit to `main`, and the change is on the live board within about
a minute. No build, no approval, and the Windows machine does not need to be switched on.

**Never edit `index.html`.** It is regenerated from a template every hour, so a change
made there is erased at the top of the next hour. That is the whole reason this file
exists. (If you do edit it by accident nothing is lost — the rebuild records who did it
and how to recover the text — but the change will not stay on the page.)

---

## `meetingNotes` — the write-up under each meeting

Keyed by the section of the page the notes belong to:

| Key | Where it appears |
|---|---|
| `mtg-notes` | Friday 17 July |
| `mtg-notes-24` | Friday 24 July |
| `mtg-notes-31` | Friday 31 July |
| `mtg-notes-0807` | Friday 7 August |
| `mtg-notes-0814` | Friday 14 August — **empty, waiting to be written up** |

Each one is a list of sections, and each section is a coloured block with a heading and
some paragraphs:

```json
"mtg-notes-0814": [
  { "title": "Sales", "accent": "teal", "items": [
      { "lead": "Where we sit", "text": "Tracking a little behind last year, with a solid quote pipeline." },
      { "text": "A paragraph with no bold lead-in is fine too — just leave 'lead' out." }
  ]}
]
```

- `title` — the heading on the block.
- `accent` — the colour stripe: `teal`, `orange`, `berry` or `green`.
- `lead` — optional. Renders bold, followed by an em dash.
- `text` — the paragraph. Basic HTML is allowed (`<strong>`, `<em>`).

To write up a new meeting, fill in the empty key. Nothing else needs to change.

## `meetingStats` — the chips in a meeting header

The numbers as presented on the day. Deliberately frozen — the hourly refresh never
touches them, which is the point: they are what was said at the time, not what is true
now. Keyed by the date exactly as it appears at the top of that meeting's section.

```json
"Friday 14 August 2026": {
  "chips": ["MTD $725k", "MTD &minus;55% vs LY", "Open $1.35M &middot; 73", "Leads 96", "Tests 1,906 YTD"],
  "note": "As presented on the day, restated ex GST so every week compares like-for-like."
}
```

An empty `chips` list hides the row, which is how an upcoming meeting sits until it has
happened.

## `gsbNotes` — the Great Southern Biology session

A flat list of `lead` + `text` items, same shape as a meeting-note section's items.

> **Editorial rule, please keep it.** That session's transcript was auto-captioned with no
> speaker labels and badly mangled species names. Nothing in these notes names an organism
> the transcript could not carry reliably — the DigestMate species list and the extra
> phosphorus-solubilising bacterium are described rather than named, until GSB confirms
> them in writing.

## `growthRoutine` — the standing social targets

The daily targets behind the growth tile in Marketing → 2026. `actions` is the list of
things counted; each needs a `k` (its internal name, must match what the numbers are
recorded against) and a `label` (what readers see).

---

## Rules that will bite if broken

- `true` and `false` are lowercase and unquoted.
- Every item in a list ends with a comma **except the last one**.
- Text goes in `"double quotes"`, never `'single'`.
- A literal `&` inside text must be written `&amp;`; an em dash is `&mdash;`, a middle dot
  `&middot;`, a minus `&minus;`.
- Straight quotes inside text need escaping (`\"`). Curly quotes (`&ldquo;` `&rdquo;`
  `&rsquo;`) are easier and look better.

## If something looks wrong

Each block falls back independently. A malformed `gsbNotes` leaves the meeting notes
alone, and any block the file does not mention keeps what it had. So a broken edit shows
up as "my change didn't appear" rather than a broken page — check commas and quotes first,
and hard-refresh (Ctrl+Shift+R) before assuming it failed.

## What is *not* in here

The margin figures. Those are measured, not written — the invoiced dollars come from Odoo
and the percentages from the costed GP sheet, on every refresh. A hand-entered value there
would be silently reapplied over fresh numbers forever, so it is deliberately left out.
The one hand-set part is the roster of names on the margin chart; ask and it can be lifted
out here too.
