# Editing the marketing calendar

The events, campaigns and posts on the dashboard's Marketing Calendar come from
**`mktcal.json` in `Hybrid-Ag/Live-Dashboard`**. Edit that published file to update
these entries without waiting for the hourly refresh. The copy in `dashboard-kit`
is the offline fallback, refreshed from the published file.

The calendar now has a year overview and a three-month focus. Events have their own
row; crop stages sit under Broadacre and Horticulture. The Broadacre soil-testing
campaign is in Cereals and continues from December into January.

Crop stages, DSA markers and timing notes live in **`marketing-calendar.js` in
`Hybrid-Ag/dashboard-kit`**. Its CSS and JavaScript are inlined by `deploy.py`, so
hourly rebuilds preserve the design and offline copies still work. The initial
reviewed window is January 2026 to June 2027. Crop windows are month-level planning
templates; the current legacy event format refers to 2026 and does not recur in 2027.

Cards with an arrow open existing content or a plan. Cards without existing work
are not interactive. Related July bud content on a spring stage does not mean that
stage's new post is complete. Stone fruit uses the supplied cherry program; its
three DSA markers stay specific to that template. Pome DSA timing is unconfirmed.

**Do not edit `index.html`.** It is generated from the `dashboard-kit` source template and
rebuilt by the hourly workflow, so any hand edit to it is erased at the top of the next hour. That is
exactly why the calendar was moved out into its own file.

## The format

One line per chip. Each line is a list of six or seven values, in this order:

```json
[7, "20 Aug", "Fruit Growers Victoria Conference", "event", "Hort · Exhibitor · McIntosh Centre, Shepparton · campaign running", false, "freegrowers"]
```

| # | Value | Notes |
|---|---|---|
| 1 | Month | `0` = January … `11` = December. The start month in 2026. |
| 2 | Date label | Free text, shown on the chip: `"20 Aug"`, `"Late Feb"`, `"Dec → early Jan"`. |
| 3 | Name | The chip's title. |
| 4 | Type | One of `event` · `campaign` · `social` · `launch` · `strategy`. Events go in the dedicated Events row. |
| 5 | Note | The small grey line under the title. |
| 6 | Done | `true` preserves completed status. It does not create a content page. |
| 7 | Page *(optional)* | Leave it off unless the chip should open an embedded page. Valid values today: `brandbook`, `wimmera`, `vicvid`, `cherry`, `frost`, `bud`, `freegrowers`, `hampers`. |

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
