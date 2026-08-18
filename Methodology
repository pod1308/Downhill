# Methodology and limits

Written so that someone hostile to your conclusion can check your work, which is
the only standard worth building to.

---

## 1. Cumulative burden

**Source.** CalEnviroScreen 4.0, published by the California Office of
Environmental Health Hazard Assessment, read directly from the state's public
ArcGIS feature service at page load. No copy is stored, so the map cannot go
stale, and the retrieval date is shown in the Data panel.

**Service.**
`services1.arcgis.com/PCHfdHz4GlDNAhBb/arcgis/rest/services/CalEnviroScreen_4_0_Results_/FeatureServer/0`

**Geography.** Census tracts. The layer has no county field, so the nine
counties are selected by FIPS code on the `tract` field, which encodes state and
county. California is 6; the county codes used are Alameda 1, Contra Costa 13,
Marin 41, Napa 55, San Francisco 75, San Mateo 81, Santa Clara 85, Solano 95 and
Sonoma 97. This is an exact filter, not a bounding box, so no tract from a
neighbouring county can leak in.

**Generalisation.** Polygons are simplified server side at roughly 50 metres to
keep the page fast. Boundaries are approximate at street scale. Scores are
unaffected.

**Tracts with no score are dropped** rather than drawn blank.

**What the number means.** Every value is a **statewide percentile**. A tract at
the 95th percentile for asthma has a higher rate than 95 percent of California
tracts. It does not mean 95 percent of residents have asthma, and it does not
mean the tract is unsafe.

**What it cannot do.** OEHHA states that CalEnviroScreen is a screening tool
designed to compare places, not to assess risk to an individual, a household or
an address. It must not be used to claim that a person's illness was caused by
where they live.

**Version note.** OEHHA has published CalEnviroScreen 5.0. This build reads the
4.0 service because that endpoint is stable and verified. 5.0's layer identifier
moved during its draft to final transition. To upgrade, change `CES_URL` and
`CES_LABEL` near the top of the script in `index.html`. The field names in the
`IND` array may also need updating if 5.0 renames indicators. The app displays
whichever label is set, so it will not claim a version it is not showing.

---

## 2. Facilities

**Source.** A fixed list embedded in the file, compiled from public regulatory
records: the five major Bay Area petroleum and renewable fuels refineries,
publicly owned treatment works discharging to the estuary, permitted landfills
and waste handling sites, power stations, and federal cleanup sites.

**What is reliable.** Names and operators. These come from public records and
can be checked.

**What is approximate.** The coordinates. They are facility centroids placed by
hand, not surveyed positions. For the large refineries they are close; for
smaller sanitary districts they may be off by a few hundred metres. Every popup
says so, and every facility links to EPA ECHO by name, which holds the
authoritative location and permit record.

**What a point means.** That a facility appears on this map means it holds or
held a permit, or is a documented cleanup site. It does **not** mean the
facility is polluting, that it is out of compliance, or that it has harmed
anything nearby.

**Coverage.** This is not a complete inventory. It covers major and
well-documented sites. Smaller permitted dischargers, air-only sources and
stormwater permittees are not included. Absence from this map is not evidence
that nothing is there.

---

## 3. Waterways

**Source.** OpenStreetMap, through the OpenMapTiles `waterway` layer in the
basemap vector tiles. The hydrology layer and the basemap are the same data, so
they cannot drift out of alignment.

**Completeness.** OSM waterway coverage is good in the Bay Area but not uniform.
Culverted and piped reaches are frequently missing because they are not visible
from the surface. A creek that appears to stop may simply go underground.

---

## 4. The downstream trace

**What it does.** Snaps the click to the nearest rendered waterway, orients that
reach toward the nearest open water reference point, then walks up to four
connected reaches, accepting each hop only when it reduces distance to open
water.

**What it is.** An indicative surface path along mapped creek geometry.

**What it is not.** A hydraulic model. It has no elevation input, no flow
volume, no velocity, and no knowledge of the piped storm drain network,
culverts, pump stations or tide gates. Real urban drainage frequently crosses
surface watershed boundaries through infrastructure this method cannot see.

**Appropriate use.** Illustrating that a location is hydrologically connected to
a receiving water, and roughly by what route. A teaching and framing tool.

**Inappropriate use.** Asserting that a discharge from a specific site reached a
specific water body. That requires dye tracing, monitoring or a calibrated
model, and this is none of those.

**Reference points.** The open water targets are five hand placed coordinates in
the Central Bay, South Bay, Lower South Bay and San Pablo Bay. They are routing
targets, never drawn, and are not a dataset.

---

## 5. Proximity and causation

The single most important limit.

When a facility appears inside a high scoring tract, three things could be true:

1. The facility contributes to that tract's burden.
2. The facility and the burden share a common cause, most often historical
   industrial zoning next to cheap flat land near water.
3. There is no relationship, and the score is driven by indicators the facility
   has nothing to do with, such as asthma rates, poverty, linguistic isolation
   or housing burden.

CalEnviroScreen combines around twenty indicators. Most of a tract's score
usually comes from indicators unrelated to any single site. **Proximity on this
map is a question, not an answer.**

To make a causal claim you need facility level discharge monitoring data,
receiving water sampling, and ideally a before and after comparison. EPA ECHO
publishes discharge monitoring reports that are the right starting point, and
the app links to them for every facility.

---

## 6. Known gaps

Stated plainly, because a methodology that lists no weaknesses has not been
examined.

- No air district monitoring data. Air indicators reach the map only indirectly,
  through CalEnviroScreen.
- No groundwater or drinking water system boundaries.
- No tribal lands layer.
- **No time dimension.** Everything is a single snapshot, so the map cannot show
  whether conditions are improving or worsening.
- Tract boundaries are generalised and unsuitable for parcel level work.
- The facility list is curated, not exhaustive, and its coordinates are
  approximate.
- CalEnviroScreen 4.0 is currently read rather than 5.0. See the version note in
  section 1.

---

## 7. Checking it yourself

Everything is verifiable without running any code.

- **The burden data.** Open the service URL in section 1 in a browser. It is a
  public ArcGIS endpoint and returns its own schema and records.
- **A specific facility.** Search its name at echo.epa.gov. That is the
  authoritative record, and the app links straight to it.
- **A specific tract score.** Search the tract at oehha.ca.gov/calenviroscreen,
  which has its own official map.
- **The waterways.** Open openstreetmap.org and compare.

If any of those disagree with this map, the other source is right and this map
is wrong. Please open an issue.
