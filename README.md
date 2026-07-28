# Development Finance Teambuilding 2026 - Desaru joining instructions

`index.html` plus a `media/` folder holding the hero video. There is no build
step and nothing to install.

> **This is no longer a single self-contained file.** The scroll-scrubbed hero
> needs `media/hero.mp4` sitting next to `index.html`, so emailing the HTML on
> its own will lose the video (the page falls back to a drawn sunrise and still
> reads correctly). To send it by email, zip the folder. On a file share or a
> static host, keep the folder structure intact.

- **Event:** Development Finance Teambuilding 2026
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

## The photographs

Five slots: the large photo in "The stay" plus a four-up gallery (beach, pool,
room, meeting space) underneath it. All five currently show real photographs of
Sand & Sandals Desaru, downloaded from the resort's own public site
(sandandsandals.com) and re-hosted in `img/` as WebP, roughly 500 KB total.
They are not hotlinked: the files live in this repository.

**These are stand-ins for the committee's own event photography, not the final
choice.** They show the real venue accurately, but nobody on the trip took
them. Swap them for the committee's own photos of this event, or the resort's,
whenever those exist, the same way you'd change any other photo:

```js
const ASSETS = {
  resortExterior: "./img/resort.jpg",     // a local file
  beachWide:      "https://.../beach.jpg", // a URL
  poolDeck:       "data:image/webp;base64,..." // or an inline image
};
```

Leave any key as `""` and that slot falls back to a drawn horizon graphic
labelled with what belongs there, so a placeholder is never mistaken for the
venue and the layout doesn't shift when a photo arrives.

**To see which slot is which**, open the page with `?assets=debug` on the end of
the URL, for example `index.html?assets=debug`. Every image slot is outlined and
labelled with its key name.

While any phone number is still missing, the browser console prints a "Not
ready to distribute" warning listing exactly what is outstanding. Press F12 to
see it.

Use WebP or compressed JPEG for anything you add. The hero video alone is
about 3.7 MB, so keep new photographs light.

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
- The footer carries an `INTERNAL - PETRONAS DEVELOPMENT FINANCE` marker.

**Send it through an internal channel** (email attachment, SharePoint, Teams)
rather than putting it on a public URL.

**On the photographs specifically:** they are the resort's own marketing
photography, downloaded from their public website rather than hotlinked. That
is a reasonable way to show attendees the actual venue while real photos are
pending, but it is still someone else's copyrighted photography being
reproduced without asking. Low risk for a small internal page that only
promotes their venue, not a concern if this stays on an internal channel, but
worth knowing before this goes anywhere more public than that, and worth
replacing with the committee's own photos once those exist regardless.

---

## Hosting on GitHub Pages

`.github/workflows/pages.yml` publishes the page on every push to this branch,
and can also be run by hand from the Actions tab.

**Pages has to be switched on once, by hand**, before the workflow can deploy.
Go to **Settings -> Pages** and set **Source** to **GitHub Actions**. Do not pick
"Deploy from a branch"; that conflicts with the workflow. Then re-run the failed
run from the Actions tab, or push any commit.

The site publishes to `https://clement119.github.io/dfn-tb-2026/`.

Two things worth knowing before that URL exists:

- **The published site is public.** GitHub Pages has no access control on a
  personal account, so anyone with the link can open it. The page is marked
  `noindex`, so it will not appear in search results, but that only makes it
  unlisted, not private. Once the organiser phone numbers are filled in, those
  become publicly readable too.
- **Pages on a private repository needs GitHub Pro or higher.** On the free plan
  the only way to publish is to make the repository public, which would also
  expose this README and the commit history.

The workflow deliberately publishes **only** `index.html`, `img/` and `media/`.
This README is excluded, because it lists the unresolved blockers and the
committee's open items and would otherwise be served at `/README.md`. The build
fails loudly if `media/hero.mp4` is missing, rather than shipping a hero that
404s.

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
- **The hero video needs H.264 support and a real network fetch.** Every current
  browser decodes H.264, but if the file is missing, blocked, or the reader has
  "reduce motion" turned on, the hero falls back to a drawn sunrise and the tall
  scroll track collapses to a normal one-screen hero. All three fallback paths
  are tested.
- **The video is about 3.7 MB**, which is most of the page's weight. Every one
  of its 97 frames is encoded as an independent keyframe rather than the usual
  mix of keyframes and predicted frames, so any scroll position can be decoded
  directly instead of walking a chain back to frame zero. That costs more file
  size than a normally-encoded clip of the same length, but it is what makes
  scroll-scrubbing look right on both desktop and mobile. It is only fetched
  when the browser can actually play it and the reader has not asked for
  reduced motion.
- **The clip is 16:9 and gets centre-cropped on phones.** In portrait you see
  roughly the middle third of the frame. If something important sits near the
  left or right edge, adjust `object-position` on `.hero__video`.

---

## How it was built

Hand-written CSS and vanilla JavaScript, no framework and no build step, so the
file keeps working wherever it is opened.

- The hero is a scroll-scrubbed timelapse. The video is never played: scroll
  position through the hero's tall track is mapped straight onto `currentTime`,
  so the clip runs forwards as you scroll down and backwards as you scroll up.
  The scrub loop only runs while the hero is on screen.
- Scroll effects use CSS scroll-driven animations with an `IntersectionObserver`
  fallback. There are no scroll event listeners.
- The render loop stops when the hero scrolls out of view or the tab is hidden.
- Type is Bricolage Grotesque (display), Switzer (body) and Chivo Mono (times and
  dates). The times are set in a monospace face so the figures line up in a
  column and the eye can scan down them.
