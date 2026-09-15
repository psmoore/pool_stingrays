# Stingrays Swim Team — Pool Relay embed preview

A replica of [swimrays.org/practice-schedule](https://www.swimrays.org/practice-schedule) with its
**five PDF buttons replaced by one live [Pool Relay](https://www.poolrelay.com) calendar** covering
all five pools.

Not an official Stingrays page. It says so in a ribbon across the top.

## The thing being demonstrated

The real page is five buttons — JRSSC, Massad YMCA, Ron Rosner YMCA, King George YMCA, Caroline
YMCA — each opening a PDF. To answer *"where is Silver on Thursday?"* you had to know which pool
Silver is in that night, open that PDF, and hope it was the current one. Nothing on the page said
when any of them last changed, and no PDF could show the team across two pools at once.

One calendar answers all five, and answers the question the five PDFs could not: **All**.

## Scoping the pool menu to five, not thirty-five

The question this page was built around: *how do you let a reader switch between five pools when
the system holds thirty-five?*

The answer is `dimFilters.location` — the tab is scoped to the 66 lane leaves under those five
facilities. The Facilities menu then offers exactly:

```
All · Jeff Rouse Swim & Sport Center · Massad Family YMCA
    · Ron Rosner Family YMCA · King George Family YMCA · Caroline Family YMCA
```

That took a product fix to be true. `resolveLocationMembers` (which draws the events) already
honored `dimFilters`, but `pageFieldOptions` (which builds the menu) did not, so the control offered
every facility in the org while the calendar could only show five. Picking one of the other thirty
was a choice that did nothing. Fixed in poolrelay, shipped, and this page is the check on it.

A second fix came out of the same page. The group filter row read
**"All | Blue | Blue | Blue | Bronze | Gold | … | Silver | Silver | Silver"** — because each
site-team has its own Manta Blue and its own Silver, which is correct (a name only has to differ
from its siblings) and unambiguous everywhere the hierarchy is drawn. The filter row is the one
place that flattens the hierarchy away. It now qualifies a name **only when it collides, and only by
as many ancestors as it takes**: *JRSSC Manta Blue*, *Massad Manta Blue* — while the *Gold* that
never clashed is still *Gold*.

## The calendar

| | |
|---|---|
| Tab | `Stingrays — practice week, all five pools` |
| Embed | `https://www.poolrelay.com/embed/lEDWU7MV8K3zxoAJxUrAmF` |
| Opens as | a **list** (`presentation.defaultPreset`), on **All** five pools |
| Scope | 66 lane leaves across five facilities; group filter pinned to Stingrays Swim Team |
| Week | 195 occurrences |

It opens as a list on a desktop too, not only a phone. Five pools in one week grid is denser than
anyone reading it needs; Day, Week and Month are one tap away for anyone who wants the grid.

The Facilities menu defaults to **All** because the page's whole claim is "one calendar for five
pools" — a calendar that opened pinned to one of them would be contradicting its own headline. In
Pool Relay an *absent* page selection means "default to the first member" and a stored empty string
means "All"; this tab stores the empty string.

## The five sites

| Site | Groups training there |
|---|---|
| Jeff Rouse Swim & Sport Center | Gold 1, Gold 2, Gold 3, Silver, Bronze, Manta |
| Massad Family YMCA | Silver, Bronze, Manta |
| Ron Rosner Family YMCA | Gold, Gold 3, Silver, Bronze, Manta |
| King George Family YMCA | Gold, Gold 3, Silver, Bronze, Manta |
| Caroline Family YMCA | Gold 3, Manta |

The schedules themselves were transcribed from the five PDFs in an earlier session; this repo is
the web page only.

## Notes

- Conflicts are off on the public calendar (`presentation.showConflicts: false`) — an overlap is an
  operator's tool, not a parent's.
- Hand-written HTML and CSS in the team's own palette (Oswald headings, `#0d47a1`, `#0b1016`
  footer). The logo is hotlinked from the team's own CDN. Off-site links open in a new tab.

## Local preview

```
python3 -m http.server 8823
```

Then open <http://localhost:8823/>.
