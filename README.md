# Downhill

A map of cumulative environmental burden and permitted dischargers across the
nine county San Francisco Bay Area.

Click any point and it reports the census tract, all of its CalEnviroScreen
percentiles, the nearest mapped waterway and the nearest permitted facility.
Click near a creek and it traces an indicative surface path downhill toward
open water.

**Live map:** https://pod1308.github.io/downhill/

---

## Publishing it: five minutes, no software to install

There is no build step, no command to run and nothing to install. `index.html`
is a single self contained file that loads its data live from California's
public CalEnviroScreen service when someone opens it.

1. Go to github.com and create a new repository called **downhill**. Make it
   public. Tick **Add a README file** so the repository is not empty.
2. Click **Add file**, then **Upload files**.
3. Drag in `index.html`, `LICENSE`, `METHODOLOGY.md`, and the `backup` folder.
   Drag this `README.md` in too and let it replace the placeholder one.
4. Click **Commit changes**.
5. Go to **Settings**, then **Pages** in the left sidebar.
6. Under **Source**, choose **Deploy from a branch**. Set branch to **main** and
   folder to **/ (root)**. Click **Save**.
7. Wait about a minute, then reload. GitHub shows the live address at the top of
   that page. Paste it into the "Live map" line above.

That is the whole process. To update anything later, upload the changed file
again and commit.

### Running it on your own computer

Double click `index.html`. It opens in your browser and works immediately. No
server, no Python, nothing to install.

---

## What the data is

| Layer | Source | How it gets here |
|---|---|---|
| Cumulative burden and 24 indicator percentiles | CalEnviroScreen, California OEHHA | Fetched live from the state's public ArcGIS service each time the page loads |
| Permitted facilities | Public regulatory records | Embedded in the file, each linking to its EPA ECHO record |
| Creeks, rivers, basemap | OpenStreetMap via OpenMapTiles and CARTO | Basemap vector tiles |

Because burden data is fetched live rather than stored, it is never stale. The
retrieval date is shown in the app's Data panel and written into every export.

If the state service is unreachable, the map says so plainly and offers a retry.
It does not substitute estimated values, because a map that quietly invents
numbers is worse than no map.

---

## What the map does not claim

This is the important part, and it is stated in the interface as well as here.

The map draws two independent public datasets on one basemap. **A facility
sitting inside or beside a high scoring tract is a fact about geography. It is
not evidence that the facility caused the score.** CalEnviroScreen combines
around twenty indicators, most of which have nothing to do with any single site.

Four specific limits:

1. **Percentiles are statewide ranks, not absolute risk.** A tract at 95 ranks
   above 95 percent of California tracts on that indicator. It does not mean
   95 percent of anything.
2. **CalEnviroScreen is a screening tool, not a health risk assessment.** OEHHA
   is explicit that it does not assess risk for an individual person or address.
   Neither does this map.
3. **Facility positions are approximate centroids**, not surveyed coordinates.
   Names and operators are reliable; the exact pin location is not. Every
   facility links to EPA ECHO, which holds the authoritative record.
4. **The downstream trace is indicative.** It follows real creek geometry but is
   not a hydraulic model and cannot see culverts, pumps or piped storm drains.
   Do not use it to establish that a discharge reached a particular water body.

Holding a permit is not a finding of harm.

If you use this in public comment or press, keep these distinctions. They are
what makes the work credible, and they are the reason an agency scientist might
be willing to engage with it.

---

## Repository layout

```
index.html            the atlas, self contained, no build step
backup/               earlier prototype with modeled demonstration layers
METHODOLOGY.md        how each number is derived, and its limits
LICENSE               MIT, for the code only
README.md             this file
```

The file in `backup/` is kept deliberately. It runs entirely offline with no
network, which makes it useful for demonstrating the interface at a meeting with
bad wifi. It contains modeled demonstration layers, clearly labeled as such
inside the app. **Use `index.html` for anything that leaves the room.**

## Licensing and attribution

The code is MIT licensed. **The data is not covered by that license** and keeps
the terms of its publishers:

- CalEnviroScreen, California OEHHA. Public domain state data. Cite OEHHA.
- Facility records, drawn from public regulatory sources. Verify in EPA ECHO.
- OpenStreetMap, © OpenStreetMap contributors, licensed ODbL. Attribution is
  required and is displayed on the map.

## Contributing

Corrections to how the data is interpreted are especially welcome. If a caveat
is missing or a claim is stated too strongly, open an issue. That matters more
here than a feature request.

If you spot a facility pin in the wrong place, open an issue with the correct
coordinates and a source. Those are hand placed and are the most likely thing to
be wrong.
