# Editing the marketing calendar

The calendar on the dashboard's Marketing tab is built from **`mktcal.json`**, in the root
of this repo. Edit that file and the change is on the live board within a minute — no
build, no waiting for the hourly refresh.

**Do not edit `index.html`.** It is generated from a template on Nathan's machine and
rebuilt every hour, so any hand edit to it is erased at the top of the next hour. That is
exactly why the calendar was moved out into its own file.

## The format

One line per chip. Each line is a list of six or seven values, in this order:

```json
[7, "20 Aug", "Fruit Growers Victoria Conference", "event", "Hort · Exhibitor · McIntosh Centre, Shepparton · campaign running", false, "freegrowers"]
```

| # | Value | Notes |
|---|---|---|
| 1 | Month | `0` = January … `11` = December. This decides which column it lands in. |
| 2 | Date label | Free text, shown on the chip: `"20 Aug"`, `"Late Feb"`, `"Dec → early Jan"`. |
| 3 | Name | The chip's title. |
| 4 | Type | One of `event` · `campaign` · `social` · `launch` · `strategy`. Sets the colour. |
| 5 | Note | The small grey line under the title. |
| 6 | Done | `true` adds the ✓ tick and greys the chip. `false` for anything upcoming. |
| 7 | Page *(optional)* | Leave it off unless the chip should open an embedded page. Valid values today: `brandbook`, `wimmera`, `vicvid`, `cherry`, `frost`, `bud`, `freegrowers`. |

Rules that will bite if broken:

- **`true` and `false` are lowercase and unquoted.** `"true"` is a string, not a boolean.
- **Every line ends with a comma except the last one.**
- Text goes in `"double quotes"`, never `'single'`.
- An `&` inside text must be written `&amp;`.
- Keep the whole entry on one line, so the change shows as a single line in the diff.

## How to make a change

1. Open `mktcal.json` on GitHub and click the pencil icon.
2. Add, edit or remove a line.
3. Commit straight to `main`. No review, no approval, nobody else involved.
4. Give it a minute — the site has to rebuild — then refresh the dashboard. If nothing
   changes, hard-refresh (Ctrl+Shift+R); the file is re-fetched on every load, so a
   stale-looking board is nearly always a cached page rather than a failed edit.

You do not need Nathan, and you do not need the Windows machine to be switched on. The
page reads this file directly, so the calendar is live whether or not the hourly rebuild
has run since.

If the JSON is malformed the calendar keeps showing its last good version rather than
breaking the page, so a typo is recoverable — but it also means a broken file looks like
"my change didn't work". If that happens, check the commas and quotes first.
