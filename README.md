# sicb_planning_output

Static pages for planning the SICB annual meeting, served with GitHub Pages. Nothing here is edited
by hand except `index.html`; the rest is generated from the meeting data by the tooling in
[`sicb_po`](https://github.com/mmchenry/sicb_po).

## Layout

```
index.html        the hub page, hand-maintained
schedule/         the room-by-time schedule grid, generated
3rd_floor.png     convention centre third floor, the 300-series breakout rooms
floor_plans.pdf   every level of the convention centre, 9 pages
.nojekyll         serve the files as-is, no Jekyll processing
```

The audience is the Programming Committee, the Divisional Program Officers included. The venue files
are linked from the hub page so a DPO can check the room layout while deciding where their division's
sessions should sit.

The schedule grid currently renders the 2026 meeting, which is a worked example for developing the
interface rather than material for the committee. Point `MEETING_YEAR` at 2027 and republish before
the site is circulated.

Each generated page is a single self-contained HTML file with no external requests, so it works from
a static host, from the filesystem, or as an email attachment.

## Rebuilding a page

From the `sicb_po` repository, in `main_schedule.ipynb`:

```python
from schedule_build import build_from_xcd, load_paths, publish_page

paths = load_paths()
build = build_from_xcd(paths, "program_sessions_085634.csv", "program_presentation_summary (12).csv")
publish_page(build, "~/code/sicb_planning_output", page="schedule", title="SICB 2026 schedule")
```

## Page contents

Each page is generated with the click-through presentation list included, so a session opens to show
its talks with presenters and affiliations. Pass `with_details=False` to `publish_page` for a page
about a third the size that keeps the grid and the conflict checks but drops the drill-down.

Note that a GitHub Pages site is served over the open internet whatever the repository's visibility
setting, unless the account is on GitHub Enterprise Cloud.

## Testing locally

```bash
cd ~/code/sicb_planning_output
python -m http.server 8000
```

Then open <http://localhost:8000>. Relative links behave the same as they will on Pages.

## Enabling GitHub Pages

Repository **Settings → Pages → Build and deployment**, source **Deploy from a branch**, branch
`main`, folder `/ (root)`. The site appears at `https://mmchenry.github.io/sicb_planning_output/`
within a minute or two of the first push.
