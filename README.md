# jcdecaux_gbfs

Daily archive of the public GBFS (General Bikeshare Feed Specification) feeds
published by JCDecaux/Cyclocity for **every city they operate bike-sharing
in worldwide** (19 systems as of 2026-07). Unlike the official feeds, which
only ever expose the *current* state, this repository accumulates history:
one snapshot every 15 minutes, forever.

## Source

Auto-discovery URL pattern: `https://api.cyclocity.fr/contracts/<contract>/gbfs/v3/gbfs.json`
(public, GBFS v3.0, no API key required). Contract list taken from
MobilityData's official GBFS systems registry
(https://github.com/MobilityData/gbfs/blob/master/systems.csv), filtered to
JCDecaux/Cyclocity-operated systems, checked 2026-07-02:

| Country | City | Contract ID | Network |
|---|---|---|---|
| BE | Namur | namur | Li Bia Velo |
| BE | Brussels | bruxelles | Villo |
| ES | Seville | seville | Sevici |
| ES | Valencia | valence | Valenbisi |
| FR | Nantes | nantes | Naolib |
| FR | Amiens | amiens | Velam |
| FR | Lyon | lyon | Vélo'v |
| FR | Cergy-Pontoise | cergy | VélO2 |
| FR | Besançon | besancon | VéloCité |
| FR | Nancy | nancy | VélOstan'lib |
| FR | Toulouse | toulouse | VélÔToulouse |
| IE | Dublin | dublin | Dublinbikes |
| JP | Toyama | toyama | CyclOcity |
| LT | Vilnius | vilnius | Cyclocity |
| LU | Luxembourg | luxembourg | Vel'OH! |
| NO | Lillestrøm | lillestrom | Bysykkel |
| SE | Lund | lund | Lundahoj |
| SI | Ljubljana | ljubljana | BicikeLJ |
| SI | Maribor | maribor | MBajk |

## Data

Each daily zip (`DD-MM-YYYY.zip`) contains, for that calendar day, flattened
files named `<contract>_<feed>_<DD-MM-YYYY_HH-MM-SS>.json`:

- **`station_status`** — fetched every 15 min. Verbatim GBFS v3
  `station_status.json` per contract: `station_id`, `num_vehicles_available`,
  `vehicle_types_available`, `num_vehicles_disabled`, `num_docks_available`,
  `num_docks_disabled`, `is_installed`, `is_renting`, `is_returning`,
  `last_reported`.
- **`station_information`** — fetched once a day (station metadata changes
  rarely). Verbatim GBFS v3 `station_information.json` per contract:
  `station_id`, `name`, `lat`, `lon`, `capacity`, etc.

Collector scripts: `gbfs_status_fetch.py`, `gbfs_info_fetch.py`,
`add_by_day.py` (shared with the sibling Valencia collectors — groups
`data/*.json` by the embedded `DD-MM-YYYY`, zips one day at a time, skips the
still-in-progress current day).

## Licence

JCDecaux's developer API is published under the **Etalab Open Licence /
Licence Ouverte** (https://github.com/etalab/licence-ouverte), which permits
redistribution, including commercial use, with attribution to JCDecaux.
