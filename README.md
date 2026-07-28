# UFR Team Building - Desaru joining instructions

A single self-contained `index.html`. Open it by double-clicking, attach it to an
email, drop it on a file share, or host it statically. There is no build step and
nothing to install.

- **Event:** UFR Team Building
- **Where:** Sand & Sandals Desaru Beach Resort & Spa, Bandar Penawar, 81930 Kota Tinggi, Johor
- **When:** 12 to 14 August 2026 (Wednesday to Friday, 3D2N)

---

## Editing the content

Everything you are likely to change lives in one block near the top of the
`<script>` tag, between these two markers:

```js
/* ============ EDIT EVERYTHING BELOW ============ */
...
/* ============ EDIT EVERYTHING ABOVE ============ */
```

You should not need to touch the HTML or the CSS.

| Object | What it controls |
|---|---|
| `EVENT` | Title, tagline, venue, address, and the start date the countdown counts to |
| `ASSETS` | The photographs (see below) |
| `AGENDA` | The three days. Each entry is `{ time, event, pic }`, plus optional `note` and `confirm` |
| `PACKING` | The checklist. `cluster` groups items under "Wear", "Bring" and "Site visit" |
| `PENGERANG` | The Pengerang site-visit block in the safety section (see "Open items") |
| `CONTACTS` | The organiser cards at the bottom |

A few conventions worth knowing:

- **`confirm: true`** on an agenda item prints a small "To confirm" chip next to
  it. Remove the flag once the detail is settled.
- **`{ gap: "In transit" }`** renders a labelled gap row. Use it instead of
  leaving an unexplained hole in the day.
- **`offsite: true`** thickens the timeline spine, used on Day 3 to mark the part
  of the day that happens away from the resort.
- Adding an item to `PACKING` automatically updates the progress counter and the
  completion moment. Nothing else needs changing.

---

## Adding the photographs

Every image slot is empty on purpose. Rather than fill them with stock beach
photos, each slot renders a drawn horizon graphic labelled with what belongs
there, so nobody mistakes a placeholder for the actual resort and the layout does
not shift when the real photo arrives.

To add photos, put a value against the matching key in `ASSETS`:

```js
const ASSETS = {
  resortExterior: "./img/resort.jpg",     // a local file
  beachWide:      "https://.../beach.jpg", // a URL
  poolDeck:       "data:image/webp;base64,..." // or an inline image
};
```

**To see which slot is which**, open the page with `?assets=debug` on the end of
the URL, for example `index.html?assets=debug`. Every image slot is outlined and
labelled with its key name.

While any photo or phone number is still missing, the browser console prints a
"Not ready to distribute" warning listing exactly what is outstanding. Press F12
to see it.

Use WebP or compressed JPEG and keep the page under about 2 MB in total.

---

## Open items

These came out of conflicts in the source material and still need a decision.
The page is built so that resolving them is a one-line edit.

| # | Item | How the page currently handles it |
|---|---|---|
| B1 | Day 3 was dated `11-Jun (Friday)` in the source agenda | Treated as a typo and corrected to **Fri 14 Aug**, which is the Friday of this trip |
| B2 | Agenda said depart 8:30 AM, the transport booking said 8:00 AM | The page shows **one** call time: report 7:30 AM, coach departs 8:00 AM |
| B3 | Agenda implies the return coach leaves Pengerang at 3:00 PM, the booking says 6:00 PM from the resort | Shows **3:00 PM from Pengerang** with a "To confirm" chip. Confirm with Theva, Ili and Akeef |
| B4 | The Pengerang visit spans Friday prayers | Not yet answered on the page. See `PENGERANG` below |
| B5 | Pengerang is a live industrial site, so PPE and site rules apply | Closed-toe shoes, long trousers and photo ID are in the checklist under "Site visit". The safety block is written but hidden |
| B6 | Unexplained gaps in Days 1 and 2 | Labelled "In transit" and "Free time" rather than left blank |
| B7 | Day 1 lunch location | Shown as "Location to be confirmed, most likely a stop en route" with a chip |

**To publish the Pengerang answer**, edit the `PENGERANG` object:

```js
const PENGERANG = {
  show: true,                       // flip this to true
  lines: [
    "Closed-toe shoes and long trousers are required on site.",
    "Bring your staff pass or photo ID.",
    "Friday prayers: <the arrangement>."
  ]
};
```

The block is already positioned in the safety section, so nothing else moves.

Still outstanding from the committee: headcount, rooming lists, organiser phone
numbers, and the event photographs.

One more thing to check: **the hero tagline is a placeholder.** The line "Three
days east, where the sun comes up out of the sea" was written to fill the hero,
not supplied by the committee. Replace `EVENT.tagline` with the real theme, or
set it to `""` to drop the line.

---

## Distribution

The content is marked confidential, so the page is built not to behave like a
public marketing site:

- `noindex, nofollow` is set, so search engines will not list it.
- There are deliberately **no** Open Graph or Twitter card tags, so a link pasted
  into a group chat will not unfurl into a preview of an internal document.
- The footer carries an `INTERNAL - PETRONAS UFR` marker.

**Send it through an internal channel** (email attachment, SharePoint, Teams)
rather than putting it on a public URL.

---

## Known limitations

- **The agenda, checklist and contact cards need JavaScript.** They are generated
  from the data block so that editing them is a one-line change. With JavaScript
  off, a short notice gives the report time, the departure time and the venue, and
  points the reader at the committee. Everything else on the page still reads.
- **Checklist ticks are not saved.** They live in memory only, so refreshing the
  page clears them. This is deliberate: `localStorage` is blocked in some embedded
  viewers. If the page ends up hosted normally and you want ticks to persist, that
  can be added in the checklist function without touching anything else.
- **The three typefaces load from Google Fonts and Fontshare.** If the network is
  slow or a corporate proxy blocks them, the page falls back to the system sans
  and stays completely readable, just less distinctive.
- **The animated hero needs WebGL.** Without it, or when the reader has "reduce
  motion" turned on, the page shows a static sunrise graphic instead. Both paths
  are tested.

---

## How it was built

Hand-written CSS and vanilla JavaScript, no framework and no build step, so the
file keeps working wherever it is opened.

- The hero is a single WebGL fragment shader written from scratch, about 8 KB, so
  there is no Three.js CDN request to fail on a weak connection. Desaru faces
  east, so it is a sunrise rather than the sunset most beach pages default to.
- Scroll effects use CSS scroll-driven animations with an `IntersectionObserver`
  fallback. There are no scroll event listeners.
- The render loop stops when the hero scrolls out of view or the tab is hidden.
- Type is Bricolage Grotesque (display), Switzer (body) and Chivo Mono (times and
  dates). The times are set in a monospace face so the figures line up in a
  column and the eye can scan down them.
