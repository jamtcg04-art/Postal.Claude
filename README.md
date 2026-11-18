# Postal.Claude

Working archive for **The Postal Trace**, a Disco Elysium-style courier RPG (browser build + Godot 4.3 port).

## Latest (Nov 17, 2026, evening)
| File | What it is |
|---|---|
| `PostalTrace_browser_V187.zip` | Full browser build (V187): `index.html`, `gaol.html`, `assets/` (3D scripts, sound, music). Open `index.html` in a browser. This is the developer build (Dev Load / Dev Mode available). |
| `PostalTrace_Godot_18.zip` | Godot 4.3 desktop project (version 18). |
| `PostalTrace_Handheld_PortMaster.zip` | Anbernic / PortMaster handheld package built from Godot 18. |
| `docs/godot-edits-folder.md` | Running notes on every edit, test and build step. |

The **phone/shareable build** lives in its own repository: `Postal.Phone` (same game, developer tools hidden, installable on a phone's home screen).

## What changed since the previous upload
- **The ferry house at Greywater Ford:** the stair and porch of the house by the water can be walked (collision all round, the floor lifts you as you climb); the door opens on the bridge gang's billet: lime plaster and red trim like the outside, no hearth (the house has no chimney), two long tables, the bridgemaster over a map, a guard at the door, Ansgar's coat on the last peg, the toll on a shelf. New conversations with the bridgemaster, the map, the peg and the toll shelf.
- **22 new movements** in both builds: the courier pulling Skelling's bell rope, saluting at first meetings, crouching or reaching to look at things; the Provost Marshal sitting at his ledger and stamping; men drinking, playing cards, laughing; a sentry with his musket carried; more idles (looking round, warming hands, a pipe, leaning on a wall); townsfolk standing in pairs talk and listen in turn.
- Godot now walks up raised floors too (the ford's bridge deck rises in the middle, as in the browser).
- Quick draw on Vane and similar cutscenes clear the UI so the 3D space is visible; time stops before the trigger with Ramrod / Cockade / Third Battery.
- Skill opinions after main and smaller choices.
- Ford sentries: rougher Crown-blue levies (forage caps, no packs, brown arm bands, the odd civilian coat), one brown coat for Marsh, blue and green bridge gang. Narrator points out the differences.
- Camp shelter: new lean-to (fire camp), brush-hidden lean-to (cold camp), tinted tent when you have a horse. Same in both builds.
- Godot camera at the ford no longer loses Marsh (BearersScene was clearing the camera target every frame).
- Browser V178–V183: horse and prop animation, road scene, phone layout.

## Older files
`PostalTrace_Dialogue.zip`, `PostalTrace_Godot 3.zip`, `PostalTrace_Godot_edits.zip`, `PostalTrace_browser_publish.zip`, `living_dice_godot.zip` and the playtest report are earlier snapshots, kept as they were.
