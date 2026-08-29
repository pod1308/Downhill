# Downhill

A map of pollution burden and permitted industrial facilities across the nine
counties of the San Francisco Bay Area.

*Website: https://pod1308.github.io/Downhill/* 

Click anywhere and it tells you which census tract you're in, every
CalEnviroScreen percentile for that tract, the nearest permitted facility and
how far away it is, and the nearest creek. Click the trace tool and it follows
the water downhill from that spot until it reaches the bay, or until the mapped
creek network runs out.

I built it because CalEnviroScreen answers "how does this tract rank against
California" and I kept wanting to ask "what is actually around this school."
Those turn out to be different questions.

## Running it

Download `index.html` and double-click it. It fetches the burden scores from 
the state when it opens.

It needs WebGL, which most browsers have and most online code playgrounds
don't. If it can't get it, the page says what's missing and falls back to a
table with the same records and the same caveats rather than showing you a
blank rectangle. 

## Where the data comes from

| Layer | Source | How it gets here |
|---|---|---|
| Burden scores and indicator percentiles | CalEnviroScreen, California OEHHA | Fetched live on every page load. Tries 5.0, falls back to 4.0, tells you which one answered |
| Permitted facilities | Public regulatory records | Embedded in the file, each linking to its EPA ECHO record |
| Creeks, rivers, basemap | OpenStreetMap via OpenMapTiles and CARTO | Basemap vector tiles |
| Places you drop | You | Your browser only, never uploaded |
| Reports you file | Your own observations | Unverified by construction, separate layer, separate export |
| Modeled proxy grid | Computed from facility positions | Nothing sampled. Violet, dashed, labelled MODELED |
| Simulated scenarios | You invent them | Off by default, kept out of every real export |

Nothing is cached. The scores come from OEHHA's service each time the page opens,
which means they can't go stale and there's no step where I could have typo'd a
number. Every tract has a link straight to OEHHA's own map so you can check it
yourself in about ten seconds.

If the state service is down, the map says so and offers a retry. It does not
fill the gap with estimates. A map that quietly invents numbers is worse than no
map.

## What it adds to CalEnviroScreen

CalEnviroScreen is the authority here and this map defers to it on every score.
What's here on top of that:

- **Places.** Drop a pin on a school and read what the public record says around
  it: tract percentile, nearest permitted facility, how many sit within three
  kilometres, distance to the nearest creek. A tract ranking doesn't tell you
  that.
- **Water.** CalEnviroScreen has no flow model. The trace shows what a spot
  drains toward, with its uncertainty spelled out.
- **County summaries.** Median percentile and the number of tracts at or above
  the statewide 90th. OEHHA doesn't publish these.
- **Your own weighting.** Re-rank the tracts by what your campaign actually cares
  about. Labelled as your index everywhere it appears, never as a
  CalEnviroScreen score.
- **Reports.** File what you observed, kept permanently separate from regulatory
  data.
- **One deliberate refusal.** The map can read both CalEnviroScreen 4.0 and 5.0,
  so drawing the change between them would be easy. It doesn't, because OEHHA
  says the tool isn't for before-and-after comparison. Details in
  METHODOLOGY.md.

## What it doesn't claim

This part matters more than the feature list, and it's in the interface as well
as here.

The map puts two independent datasets on one basemap. A facility sitting inside a
high-scoring tract is a fact about geography. It is not evidence that the
facility caused the score. CalEnviroScreen combines around twenty indicators and
most of them have nothing to do with any particular site.

Specifically:

1. **Percentiles are statewide ranks, not absolute risk.** A tract at 95 ranks
   above 95 percent of California tracts on that indicator. It doesn't mean 95
   percent of anything.
2. **It's a screening tool, not a health risk assessment.** OEHHA is explicit
   that CalEnviroScreen doesn't assess risk for an individual person or address.
   Neither does this.
3. **Facility pins are approximate.** Names and operators come from public
   records and are reliable. The coordinates are centroids I placed by hand and
   are not. Every facility links to EPA ECHO, which has the real location.

   On those links: EPA addresses each facility report by a Facility Registry
   Service ID, and a wrong ID opens some other company's compliance file. So the
   map never guesses one. Two are confirmed and hardcoded; the rest are looked up
   from EPA when you click, and if several sites match you get the candidates and
   pick. The Data panel shows the ratio so it isn't hidden. Confirming more takes
   about a minute each and is the single most useful thing anyone could
   contribute.
4. **The trace makes two different claims.** The amber dashed leg from your click
   to the nearest creek is inferred. With no elevation model it tells you how far
   the nearest channel is, not the path water takes. The solid leg follows real
   OpenStreetMap geometry, using OSM's convention that a waterway is drawn in the
   direction it flows. If the mapped network ends before open water, usually
   because the creek goes into a culvert, the map says so and stops instead of
   drawing a line to a bay it never reached.
5. **The simulator is a toy.** Distance decay and nothing else: no monitoring
   input, no dispersion physics, no units. It's off until you place something, it
   draws in colours no real layer uses, everything it produces is tagged, and it
   never touches the real exports. Its own file is called
   `SIMULATED-scenario.geojson` and carries a warning inside. Don't quote a
   number from it.
6. **The proxy grid was not sampled.** No monitor, no lab, no field visit. It's
   arithmetic over facility positions showing where permitted industry clusters.
   Not contamination, not exposure.

Holding a permit is not a finding of harm.

If you're using any of this in a public comment or talking to a reporter, keep
these distinctions. They're what makes it credible, and they're the reason
someone at an agency might engage with it rather than dismiss it.

## What's in the repo

```
index.html         the map, self-contained
METHODOLOGY.md     where every number comes from and what it can't support
```

`backup/` runs with no network at all, which makes it handy for demoing at a
meeting with bad wifi. It contains modeled demonstration layers, labelled as such
inside the app. Use `index.html` for anything that leaves the room.

## Licence and attribution

Code is MIT. **The data isn't mine to license** and keeps its publishers' terms:

- CalEnviroScreen, California OEHHA. Public state data. Cite OEHHA.
- Facility records from public regulatory sources. Verify in EPA ECHO.
- OpenStreetMap, © OpenStreetMap contributors, ODbL. Attribution required, and
  it's displayed on the map.

## Contributing

Corrections to how the data is interpreted are more welcome than features. If a
caveat is missing, or I've stated something more strongly than the data supports,
please open an issue. That's the kind of bug that actually matters here.

If a facility pin is in the wrong place, open an issue with the right
coordinates and a source. Those are hand-placed and they're the likeliest thing
on the map to be wrong.
