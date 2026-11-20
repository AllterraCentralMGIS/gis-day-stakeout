# Stakeout — GIS Day setup

One file: `index.html`. Edit only the `CONFIG` block at the top.

## 1. Locations layer — done 2026-11-19

- **Game_Locations_1** is shared publicly. Confirmed 2026-11-19.
- The app reads only **OBJECTID 3–7** (Houston set, Location_Number 1–5).
  The other 8 points in the layer (OBJECTID 8–15) are ignored.
- **To do:** turn editing **off** on this layer (Settings → Editing).
  It is public with Create/Update/Delete on, so anyone with the URL could move the points.

## 2. Scores table — done 2026-11-19

- **GIS_Day_Scores** is live, public, Add-only, and wired into `SCORES_URL`.
- Test save confirmed 2026-11-19: one row, OBJECTID 1, team `TEST`, 0 pts.
  The leaderboard ignores it. **Delete it in ArcGIS Online before GIS Day** (Data tab → select row → delete).

How it was built, for next time:

1. Content → New item → **Feature layer** → *Define your own layer* → **Table** (no geometry).
   Name it `GIS_Day_Scores`.
2. Add fields:

   | Field        | Type    |
   |--------------|---------|
   | `team`       | String  |
   | `station`    | String  |
   | `points`     | Integer |
   | `entered_by` | String  |

3. Item → Settings → **Enable editing** → allow **Add** only (turn off Update and Delete).
4. Share → **Everyone (public)**.
5. Copy the table's URL ending in `/FeatureServer/0` into `SCORES_URL`.

Add-only means nobody can change or delete a saved score from the app.
To fix a bad score, edit the table yourself in ArcGIS Online.

## 3. Fill in CONFIG

- **Stations are already mapped** (order = rotation order):

  Source: the paper worksheet, confirmed by Miguel 2026-11-19. It replaced an earlier order
  taken from the layer's Game_type field.

  | Spot | OBJECTID | Game | Scoring |
  |---|---|---|---|
  | 1 | 3 | Geography: highest waterfall (Venezuela) | 5 pts if correct, auto |
  | 2 | 4 | Cornhole, 1 minute | Host enters points (5 hole, 3 board) |
  | 3 | 5 | Geography: Equator circumference (25,000 mi) | 5 pts if correct, auto |
  | 4 | 6 | Smarties, timed | Host enters time; fastest 10, then 8, 6, 4, 2 |
  | 5 | 7 | Geography: not an ocean zone (Coastal) | 5 pts if correct, auto |

  **To do:** fix Game_type in the layer to match (OBJECTID 4 → Cornhole, 6 → Smarties, 7 → Geography).
  The app ignores that field, so this only affects your map.

  Smarties notes: the latest time entered for a group counts. Tied times get the same points.
  Points shift as more groups finish, and are final once all 5 times are in.
- **`phraseHash`**: open the app → Host tab → *Make a hash* → type the phrase → *As phrase*.
  Paste the result. Phrases ignore case, spaces and punctuation.
- **`HOST_PIN_HASH`**: same tool, *As PIN*. Default PIN is `1234` — change it.
- **`questions`**: the geography questions are samples. Replace them.
  `answer` is the position of the right choice, starting at 0.
- **`NAME_FIELD`**: leave blank. The layer has no name field, so the app shows the station titles.

## 4. Rotation

Group 1 starts at station 1, Group 2 at station 2, and so on.
Everyone moves to the next station in order, so no two groups share a spot.

## 5. Rehearse

Add `?demo=1` to the URL. Fake points, demo phrase is `demo`, scores stay on that phone.

## 6. Host it

Any static host works: GitHub Pages, Netlify, or your org's web server.
It must be served over **https** so phones allow GPS.
