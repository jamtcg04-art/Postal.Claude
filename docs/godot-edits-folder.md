
## Edits files read straight from the browser (5 November) — Godot
- Built on the pass 13 project the user uploaded 5 Nov (PostalTrace_Godot_3.zip); first delivered as PostalTrace_Godot_edits.zip, re-applied in PostalTrace_Godot_4.zip (7 Nov).
- scripts/world/place_edits.gd: refresh_drops() reads every *.json in res://edits/ (in the project; added to both export presets' include_filter), user://edits/, and <executable dir>/edits/ in exported builds. It runs from for_key/export_text at most once a second (so whenever a place is built) or at once from the editor's **Check edits folder** button (plus **Open edits folder**).
- Newest wins per place: each file's own "at" time (else its modified time) against a per-place stamp in user://place_edits_meta.json; save_user stamps every place whose edits changed. A taken place is copied into the user store, so the Godot editor carries on from it; a file is taken again only if its contents change (length + hash recorded). Non-edits files are logged and skipped (PlaceEdits.drop_log, console).
- data/place_edits.json (baked) still works as the lowest layer.
- tests/drop_edits_test.gd 8/8 (fixture tests/fixtures/browser_place_edits.json, made with the live browser editor: crossroads well moved/turned/darkened, a lantern added).

## Handheld: sharp name tags, remappable pad buttons (7 November) — Godot (PostalTrace_Godot_4.zip)
- Name tags and passing remarks were Label3Ds inside the half-size 3D view (world_shrink 2 on hh), so they came out as blocks. main.gd now hides those (layers 0) when world_shrink > 1 and draws two Labels (world_tags over the world view) at the projected spot, at full resolution, Caladea with outline, 18/16 px × Settings text size. Desktop unchanged.
- scripts/pad.gd: every command except the d-pad is remappable (Menu > Controls, pad section first on hh): ACTS defaults + settings.cfg [padmap] cmd = [codes] (button index, or "a<axis>±" for triggers/sticks). New commands: hh_zin / hh_zout (zoom, repeat while held; unbound by default, Select + L1/R1 still zooms), hh_run (hold), hh_char / hh_tin / hh_bag / hh_log (open/close that page). Pad.bind() moves a button off whatever had it; hh_ok, hh_back, hh_menu can't be left without one; d-pad can't be taken. start_capture(cmd) → next pad press binds; captured signal reopens Controls with a message. "Pad buttons back to the usual" (reset_map). A/B swap still applies to defaults.
- Pad hint bar and "Continue (A)" use the current buttons and leave out commands with none.
- User asked whether turning instead of zoom on the handheld was a limitation: no — L1/R1 turn the view by design; Select + L1/R1 zooms; zoom can now be put on its own buttons.
- Tests: pad_map_test 15/15 (PT_HANDHELD=1), handheld 21/21, touch 15/15, saves 16/16, editor 18/18, edits 11/11, drop_edits 8/8, systems pass. 640×480 shots (xvfb, gl_compatibility) before/after the tag fix.

## Journal: walk to the person before talking (8 November) — both builds
- Picking someone in the journal / "Who is here" list with 3D on asks "Walk to and talk to <name>?". Yes (E/Enter, or the pad's Pick button) closes the prompt, walks over (browser: closes the journal, pl.goal with then → act(); Godot: world.approach(pick), handheld switches to walk mode) and the conversation opens on arrival. No (Esc, or the pad's Back) leaves things as they were. 3D off, the person not standing in the place, or a companion at your side: talks at once as before.
- Fixed a browser bug found on the way: the journal's buttons carry an index from when the page was drawn, and the market's talker list can change before the click (people on their rounds), so picking Mother Quill opened Magpie. Both builds now find the person again at the click (browser by name, Godot by id).
- Browser: published as artifact version 1794183138-c9ed (V170 block before wJournal; capture-phase click on #screen [data-tk], #wask dialog). Godot: main.gd talk_pick_id / ask_walk / ask_yes / ask_no (modal_tag "ask"), PostalTrace_Godot_5.zip. tests/journal_walk_test.gd 9/9 (also under PT_HANDHELD=1 with the pad); regressions pass.

## One on one with the voices; voice sounds fixed (11 November) — all three builds
- dialogue/inner.pt: nine Disco-style conversations with a single voice (inner:x_in_<id>, every node @by): the dispatch case (Cipher), a revolver (Old Shoulder), the Third Battery's letters (the Battery), leaving the redoubt (the Mud, then the Long Road), leaving the signal hut (the Wire), walking into the Bell (the Splinter), after the Quarter Cut leave (Old Shoulder), after selling the deserters (Ledger), stamina 2 or less (the Long Road). Three new thoughts to mull: honest_ground "The Ground Is Honest", seal_weight "The Weight of Wax", hum_after "The Hum After" (browser THOUGHTS + Godot tables.json).
- What opens them: dialogue/inner.json (copied to data/inner.json; embedded in the browser's V171 block). item → "Listen to it (‹voice›)" in the satchel (not in a fight); leave → plays when you leave that place, then the walk to the edge, fade to black, road; enter "place:area" → offered 0.9 s after going through that door; flag(+eq)/stat+max → offered once true. Offers sit on the voice ring (V / pad voice button opens the conversation) and as a "‹Voice› wants a word" button on the place's screen. Each plays once (done.in_heard_<id>); memory keys inner_x_in_<id>_… in both builds.
- Godot gained the browser's walk-out on leaving a place (main._exit_then: walk to the place's exit, fade at 1.3 s, road at 2.4 s; skipped headless unless PT_CUT=1, or with 3D off).
- Sounds (Godot/handheld): sound.gd now follows the browser: per-sound players that restart (sfx), the browser's volume table and fail variants, Sound.event() priority queue (40 ms, most important wins), voices on their own rules (duty/ringing/hat own files, others v_<voice>; 8 s step-down to a third; lines already heard silent via S.bseen; first voice of a moment only). Conversation voice lines sound as each is revealed with Continue, not all at once when the screen is built; the duplicate sting in bridge voiceBlock and the per-screen sting were removed; ringing now has its sound.
- Browser published as version 1794421548-988d. Godot: PostalTrace_Godot_7.zip; handheld postaltrace.zip rebuilt. tests/inner_test.gd 21/21 (also PT_HANDHELD=1); browser t_inner.py 16/16; regressions pass (journal 6/6 and 9/9 hh, saves 16/16, touch 15/15, editor 18/18, edits 11/11, drop edits 8/8, clicks 2/2, handheld 21/21, pad map 15/15, systems 0 fails; dialogue parity 320/321, the one difference is a coin count in the dice game, not these changes).

## The Walking Lead, Hale canon, voice rules (14 November) — all three builds
- Hale text fixes (browser + tables.json/lib.json/scenes): mother not widow; "of the Fourth Line"; ten bearers plus Dunmore's own name; letter "did not die on the pass", one line scored out; hat "I would rather the pass" + (carried) "Then whose head was under me?…"; Dunmore sleep-talk on the stand-watch success text.
- Hole and betrayal clues in bearers.pt (Cipher 2+ after the letter, Splinter 2+ hole height, Wire 2+ hand to his head, Ledger/Long Road argue on the platoon numbers); new inner conversations: pass (Cockade, suspects Venn/Skerry and Pike/Teodric/nobody, never confirmed), map (Cipher, the pencil crosses its sevens).
- Walking Lead: back_oak.pt surgery (surgeon rolls 2d6+3 vs 12; player modifiers capped +3: hold still Grit 8 red, Splinter Arms 9 red, lamp 2 coin, brandy; −1 worn through, −1 Ringing at 3+ memories; Blackthorne's probe +1 outside the cap; one attempt). Blackthorne letter → reply two stops later (inner bt_reply, item bt_probe). Fail → the ball comes out at the next stop (inner lead_supper). Item lead_ball (carried: full Splinter voice); let it go at the bearers' box, the river or Redoubt Nine → done.lead_echo, Splinter lines swapped for SPLINTER_ECHO (echoText in voiceBlock / Game.voice_text).
- New inner trigger "fn" (btReady, leadSupper) in both builds; inner conversations now open through graphGo/_graph_go so hub @flag/@give apply.
- Browser V176 (version 1794699135-4657). PostalTrace_Godot_8.zip; handheld postaltrace.zip rebuilt. Tests: lead_test 14/14 (also hh), inner 21/21 (also hh), browser t_lead 10/10, t_more 10/10, t_inner 16/16, saves 16/16, journal 6/6 + 9/9 hh, touch 15/15, editor 18/18, edits 11/11, drop edits 8/8, handheld 21/21, pad map 15/15, clicks 2/2, systems 0 fails, dialogue parity 319/319 (fixture re-recorded; inner conversations are not recordable by parity_record and were left out).

## Animations, phases 1 and 2 (14 November) — Godot only (browser to follow later)
- User asked for horse, cart/wheel, bearer, door, lantern, riding, mounted/foot combat and scene animations (Vane's quick draw, the Oakhaven surgery); Godot first, browser later; use the game's own models, placeholders only if nothing fits.
- Found: assets/3d/people/person.glb already has a 22-bone rig with 39 movements (idle, walk, carry, kneel, drive, pidle/pshoot/paim/preload, sword/slash/block, punch, hitc/hith, death...). Horses, carts, doors and lamps are single static meshes.
- scripts/world/prop_parts.gd (PropParts): splits a model's own mesh into moving parts by geometry (welded components), per data/prop_parts.json: wheels of cart, mk_wagon, mn_cart_a/b/c (found by tools: anim/wheels.py logic), and b_lamp's hanging head. Cached per model.
- scripts/world/prop_motion.gd (PropMotion, made by fx.gd for every place): outdoor b_lamp heads and high lanterns swing in a gusty wind; door models within 1.6 m of a doorway (tavern bedroom door, bell door) swing open away from the courier on going through (0.4 s, door_open sound), and the door you came through swings shut behind (door_close); building doors: the courier pushes, door sound, then the next place. roll(cart, dist) turns wheels; drive([horse, cart, ...], path, speed) moves a team, horse pulls/walks/trots/gallops by speed, wheels turn with distance.
- tools/anim/rig_horse.py: writes assets/3d/models/cp_horse_rig.glb and cp_horse_grey_rig.glb from the game's horse meshes: 20 bones (root, hips, chest, neck1/2, head, tail1/2, 3 per leg) + saddle bone; weights by geometry; movements idle, walk, pull, trot, gallop, rear, shy (plain glTF, Blender can edit). Imported with remove_immutable_tracks=false. data/models.json sizes for *_rig.
- Every horse prop now carries its rig (PropMotion.rig_horse, in the prop's tint), idle at a different point per horse; horse_play(), horse_anim(), saddle() (BoneAttachment3D on the saddle bone). Figure.play_once() + hold for one-off movements.
- Tests: tests/anim_test.gd 16/16; horses_test 11/11 (check updated for rigs); regressions pass (saves 16/16, journal 6/6 + 9/9 hh, touch 15/15, editor 18/18, edits 11/11, drop edits 8/8, clicks 2/2, inner 21/21, lead 14/14, walkers 9/9, places OK 38, handheld 21/21, pad map 15/15, systems 0 fails). Shots: tests/anim_shots.gd, tests/horse_shots.gd. PostalTrace_Godot_9.zip, postaltrace.zip.
- Phase 3 still to do: riding (mount/dismount, riding pose), bearers carry/drop/guard the box in the ambush, fights staged in 3D on foot and mounted, Vane's quick draw, the Oakhaven surgery.

## Road scene, camps, patrols at camp, thieves, riding, 3D fights, set pieces (14 November)
- Mechanics in BOTH builds (browser V177 + data/lib.json, same code): "Rest 3 hours" is now "Make camp": campShow — fire camp (tent off the saddle with a horse, lean-to without) or a cold, hidden camp; Notice ≥ 4 forces the cold camp. Patrol finds the camp: fire 8% × Notice (+2% with a horse), cold 4% × Notice, halved with the notice perk, max 60%; campFound (warrant: Nerve 9; slip away: Grit 8, 7 for a cold camp; ride for it; bribe 2; fight) → campFight (corporal + 2 provost men, loss = arrest). Thieves: roadHook wrapper, 15% per leg after other events, max 3 per run; thievesEvent (face them down Nerve 7, run Grit 7, ride, 1 coin, fight) → thievesFight (cudgel, knife, sometimes a boy with stones; melee only, panic 0.35; losing halves your coin, never kills). S.camp {kind, shelter, horse, idx} for the 3D view. Browser: rest button relabelled; road talks skipped in innerCheck (x.at === 'road').
- Godot road scene (scripts/world/road_view.gd, world.enter_road/road_set, main road_on): between stops the courier walks (or rides) in place on a looping road with roadside trees, bushes, rocks and fences from the game's models; stops for encounters by screen title (thieves step out of the hedge, provost pole, hunting riders, rider down, broken wheel, dead horse, farm fire, foragers); camp built by the road (tent / lean-to / brush-covered cold camp, fire with glow, horse tethered, courier sits or lies low), night lighting; "Go on" walks on and fades to the next place. Four road talks (inner.pt, "at": "road"): road_first (Long Road), road_notice (Wire, Notice ≥ 3), road_horse (Third Battery, with a horse), road_cold (Mud, after a cold camp); dropped from the queue when the next place comes.
- Riding: Figure.mount()/dismount() with a RidePose SkeletonModifier3D (the rig's drive pose, thighs down and apart: RIDE thigh_spread 0.3, thigh_down 0.7, calf −0.5), hips kept on the horse's saddle bone (PropMotion.saddle_xform). Road: in the saddle, down at camp. Places: the courier's horse (HORSES id → model/tint) appears outdoors, ridden in from the road (S._from_road), trot 5 m/s / gallop 9 m/s; gets down to talk, look or go through a door; click the horse ("ride …") to mount again.
- 3D fights (scripts/world/fight_stage.gd): Godot now answers the browser's W.stage events (spawn, round, shot, foeShot, foeHit, lunge, hurt, down, flee, ally, blast, unhorse, reset, despawn): men spawned round the courier, riders on rigged horses, brawlers close in, gunmen keep range, flashes, blows, reels, falls, flight; thieves never carry guns. The place now stays up behind fight screens (W.keep, as in the browser).
- Bearers' ambush (scripts/world/bearers_scene.gd): ride to the ford (the fight's spawn plays the column rolling with the courier on the cart, then rebels and the rider on the grey, Pike and Skerry; the fight uses those men); watch fails (night) and warning ignored (resolve hook) play the hit where they stand. Every time the box comes down off the cart and the bearers kneel round it facing out; volleys; the camera holds on the cart (world.look_here).
- Scenes (scripts/world/scenes3d.gd): Vane's quick draw on "Clear the road with iron and powder" (standoff, draw, Vane drops and his men run; or the ball goes wide, the axe, the fowling piece, then they give way); the Oakhaven surgery follows the dialogue nodes (lead_go: on a cot under the canvas; probe; the ball held up, or a shake of the head; up again on the hub).
- Tests: road 11/11, camp 9/9, ride 7/7, fight 4/4, bearers 6/6, scenes 6/6 (+ shots: road_shots, ride_shots, fight_shots, bearers_shots, scenes_shots); browser t_camp 9/9.

## Animation, the 3D road and the set pieces brought to the browser (15 November) — browser V178, Godot 11
- The user asked for the Godot-only animation work in the other builds, "at least the artifact version". Browser V178 (version 1794733818-14d1) now has all of it, with the same bones, numbers and layouts as Godot. The handheld already had it (it is the Godot build).
- New asset assets/w3d_anim.js (loaded after w3d_core.js):
  - W3D.horseRig: made by port/horse_rig_js.py from tools/anim/rig_horse.py, with the same 20 bones, weights and movements (idle, walk, pull, trot, gallop, rear, shy) at 20 fps.
  - W3D.propSplit: made by port/wheels.py with Godot's prop_parts.json test. It marks which triangles of mk_wagon and mn_cart_a/b/c are wheels.
- Horses (wHorseDraw): every cp_horse or cp_horse_grey prop is drawn skinned with the people's RG shader (horseAnim's head toss turns off once the rig is up).
  - The gait comes from speed: under 0.3 idle; under 2.8 walk; under 5.4 trot; otherwise gallop. A harnessed horse (it.pull) pulls.
  - it.hclip/it.hrate force a movement; it.hact does a one-off (wHorseAct).
  - it._sad holds the saddle in the world. A rider (pl.rideOn, or a mounted extra with e.horse) is drawn in the drive pose with Godot's RIDE leg turns, hips on the saddle (wRigDraw).
- Props (wPropDraw, wPropList):
  - Wagon and cart wheels turn with the distance rolled.
  - Outdoor b_lamp heads and high lanterns swing in Godot's wind.
  - Door models (f_door, f_doortall, bell_door, bell_rooms_door) swing open away from you as you go through (wGoArea waits 0.38 s), and the one by your arrival point swings shut.
- Your horse in places (myHorseTick): outdoors, from HORSE_LOOK. You arrive in the saddle from the road (S._from_road) and move ×1.2 mounted. You get down to talk or look (frozen) or to go through a door. Clicking the horse ("Get up in the saddle") mounts you again.
- Road scene (WORLDS['@road'], roadBuild/roadSet/roadTick): the same tile, side-prop and encounter layouts as road_view.gd.
  - The ground scrolls through g.flowOff, and side props scroll and respawn. The courier walks in place, or rides with a horse.
  - The road stays up behind every screen while W.roadOn (show() swaps W.keep to '@road', so fights happen on the road). The mode comes from the screen titles (RD_TITLES) as in Godot.
  - Camps: tent, lean-to, or a brush-covered cold camp; fire and lights; night fog and light; the horse tethered.
  - "Go on" walks on, fades out (0.7 + 0.8 s), then scene() and a fade back in.
  - roadLeave() runs at the top of scene(), in ending() and in revisit(), and drops the road talks. start() clears it.
  - Road talks: innerCheck(at) now matches x.at exactly; road() offers the at:'road' talks; a talk opened on the road returns to the road.
- Set pieces:
  - The bearers' hit (resolve hook: bearers choices 0/1 failed, night for the watch): rebels walk in; the rider trots out on the rigged grey; the box comes down off the cart; the line kneels round it with muskets; 6 s of volleys; most rebels then flee; the camera holds on the cart for 9 s.
  - The quick draw (cross, choice 0).
  - The surgery cot and lying pose (graphShow hook on lead_go, lead_probe, lead_ok, lead_bad, lead_bad2; up on hub or scene).
- Both builds:
  - Sitting at the fire camp is now beside the fire, (1.8, −2.5), turned to it (it was on top of it).
  - A voice talking on the road keeps the courier walking (or by the camp). In Godot this is _road_mode_for with inner_on.
- Deliverables: PostalTrace_Godot_11.zip and a rebuilt postaltrace.zip (handheld pck).
- Tests:
  - Browser: t_anim 14/14, t_scenes 8/8, t_props 2/2, t_roadvoice 4/4, t_lead 10/10, t_more 10/10, t_camp 9/9, t_inner2 16/16.
  - Godot: road 11/11, camp 9/9, ride 7/7, inner 21/21 (PT_CUT=1), anim 16/16, horses 11/11, fight 4/4, bearers 6/6, scenes 6/6, lead 14/14, places OK 38, handheld 21/21 (PT_HANDHELD=1).

## Time stops, cutscenes without the story panel, and the voices' verdicts (15 November): browser V179, Godot 12
- User asked for three things:
  - cutscenes like the quick draw with nothing covering the 3D view;
  - a Disco-style time stop right before the trigger, where the Ramrod (thrill of powder), the Cockade (necessary, with the false hope the Crown will thank you) and the Third Battery (neither disappointed nor proud; Vane is a man surviving, not a soldier) argue;
  - skill opinions on the player's main choices, and smaller ones later if possible.
- Shared data (browser `var REACT`, Godot tables.json `TIMESTOP` and `VERDICTS`; source port/react.json):
  - `TIMESTOP["scene:choiceIdx"]` holds {title, lines [[voice, text, min?]...], go}.
  - `VERDICTS[scene][choiceIdx]` holds {ok, bad, or all: [[voice, text, min?]...]}.
  - Lines are gated by voiceOn and mutedBy.
  - The Cockade and the hat are left out of verdicts because the existing choice voices (SCENE_VOICE and the tallies) already give their word.
- Flow for "Clear the road with iron and powder" at Cross-Pillars, the same in both builds:
  - The choice is intercepted (browser: a resolve wrapper; Godot: `_choose` from the scene's choice buttons).
  - The story is put away (browser: W.cutFn and wcut letterbox; Godot: `cine(true)`, which hides scroll and panel_bg, shows black bars and makes world_view full).
  - Stand-off: the courier walks out to 4 m from Vane at 3 m/s; Vane squares up in pidle; the fowler aims; the camera centres between them and zooms to 0.62×. 1.3 s after arriving the pistol comes up (paim); 0.8 s later time stops.
  - Time stop: the world freezes (browser: W.timeStop, dt 0, ts frozen; Godot: world.process_mode DISABLED) and turns grey (CSS filter or a canvas shader). Voice lines come one at a time in a box over the view (Space, Enter, click, or the pad's A); the last button reads "Pull the trigger".
  - Then resolve rolls, and the shot plays with the story still away (quickdraw retimed: shot at 0.25 s, 3.4 s on a hit, 4.2 s on a miss; returns its length for `cine_for`). The result comes back with the verdict voices under it.
- The story is also kept out of the way for the bearers' hit (8 s) and, in Godot, for the ride to the ford (10.4 s; the browser's bearersRide already used a cut).
- Verdicts are written so far for Cross-Pillars only (all five choices, success and failure) as the sample for the user's review. The other main stops and the smaller choices wait on their go-ahead.
- Tests:
  - New: browser t_timestop 8/8; Godot timestop_test 6/6 (PT_CUT=1).
  - Shots: tests/timestop_shots.gd.
  - Regressions: browser t_anim 14/14, t_scenes 8/8, t_lead 10/10, t_more 10/10, t_camp 9/9.
- Update, later on 15 November: the user approved the Cross-Pillars sample ("Yes on all the above"), so this is now done in both builds (browser V180, Godot 13):
  - Verdicts for every main stop: ford, oak, ridge, malice, mill, pit, siblings, hermits, bearers, venn, chapel, market, mine and wirehut, in REACT.VERDICTS and tables.json.
  - Smaller choices: smallVerdict(o, bad) is a shared function, in the browser and in lib.json for Godot, with its data inline and keyed by the option's own words (a trailing * matches a prefix). It is called from evPick and rdPick under the result. It covers 80 options:
    - the camp, a patrol finding the camp, the thieves, the provost patrol, the hunting party;
    - every road event (the rider, the bridge, the wheel, the dead horse, the barn, the fire, the foragers), the valley's help, the provost's cart, the sky-fire;
    - the errands (the keeper's bread, flour, Bet's letter, the commander, the colours, the mill's debt and Venn's note, the fever root, the pigeons, the brandy and the keg, Edrik's tin, the slow fuse and the spare charges, the deserters' roads, the evening gun, Merrow's offices, the crew's horses, Bess, Vane's black, the ferry lamp).
    - In the small verdicts the Cockade speaks too, since there is no tally voice there; the hat is left out.
  - More time stops:
    - oak:3, the shot over the mutineers' heads (a gun stop);
    - ridge:1, the patrol (a gun stop);
    - pit:1, Hesk's pistol;
    - venn:2, selling the summary;
    - malice:0–3, the last choice: deliver, tell the garrison, burn, sell;
    - bearers:ambush, as the treeline moves at the end of the ride to the ford, before the fight (browser: bearersRide's cut callback calls tsMoment; Godot: BearersScene.ride calls main.time_stop_key).
    - A stop with no staged scene uses the generic build-up: the camera comes in on the courier at 0.7× zoom. A gun stop also raises the pistol, and after the voices the shot fires with a flash before the result.
  - Godot: main._side() now leaves the panel hidden while a cutscene runs (`_cine_on`), so a fight screen opening during the ride does not bring the story back early. BearersScene finds the main screen up the tree (`_main()`).
  - Tests: browser t_verdicts 9/9; Godot timestop2_test 6/6.

## Uniforms: Ostrey's levies, the bridge gang, soldiers no longer in dresses (15 November): browser V181, Godot 14
- What the user spotted: the levies across the ford wore green while Captain Ostrey wore Crown blue, and some of them were women.
  - The green was hand-set in the ford's build code when the place was made, before the uniform system gave green to deserters. In the dialogue "the green" means the rebels.
  - The women came from the V164 everyday-clothes system, which dressed every unnamed extra as a townsperson, with a share of women. The same happened to Hale's bearers, the soldiers at Redoubt Nine, the gun-pit, and the Fort Malice and Oakhaven guards.
  - ford.pt also called Ostrey's own coat green.
- The user's decisions:
  - Levies wear Crown blue, rougher: forage caps, no packs, the odd civilian coat, brown armbands.
  - Marsh, made corporal on Tuesday, wears a brown coat.
  - The narrator points out the differences.
  - The bridge gang is a mix of blue and green.
- Done (V181 block at the end of index.html, a wBuild wrapper):
  - **Who counts as a soldier:** extras with no id get the line uniform, wRigUniform('pike'), when the layout gives them hue 2.1 (the layouts' convention for soldiers) and everywhere at the redoubt and the pit.
  - **The levies:** rgLevy('blue') is soldier_g/blue with the shako and pack dropped, plus a cap, a brown capband and a brown armband (LEVY_BAND 0.4,0.27,0.14). Three of the eleven wear a farm coat (wFitFor coat, male) with the cap and the brown band.
  - **Marsh:** the 'sentries' talker, and the lookout extra on the way back, wear MARSH_COAT brown (0.38,0.26,0.15).
  - **The bridge gang:** three new extras by the bridge: green d1, blue 'bridge', green d2 with a club.
  - **ford.pt narration:**
    - Ostrey's coat is "the only full regimental coat at the post: Crown blue, facings whole".
    - The levies are described as "made do with": caps, no packs, a brown strip where facings would be, farm coats.
    - At Redoubt Six: two in blue, one in a farmer's brown coat.
    - Marsh: "The blue one went with the knot’s last owner… Captain says it’s the knot that matters, not the cloth."
- Godot:
  - places_export.js was re-run on the browser build. The extras arrays (and ford's people and solids) were copied into data/places for bearers, ford, malice (+ cells, hall, hq, stores, ward), oak, pit and redoubt. Market was left alone; it only differs in walker randomness.
  - Those places were re-baked with tools/bake_places.gd. Their old .tscn files were pure bakes, with the same timestamps as the json.
- Tests:
  - New: browser t_levies 5/5.
  - Dialogue parity still 319/319.
  - Shots: tests/levy_shots.gd.

## V182–V183 / Godot 15 (Nov 17)
- Camp shelter: new `cp_leanto` (forked uprights, ridge pole, bough thatch, greatcoat). A fire camp gets the lean-to; the cold camp gets the lean-to under brush; with a horse it's the tent (tinted drab when hidden). Same in road_view.gd.
- Marsh at the ford was already in the same spot in both builds; Godot's camera lost its framing because BearersScene cleared `look_here` every frame (`_cam_mine` fix).
- Phone layout (browser V183 block): on touch the help line reads "Tap to walk · tap twice to run · pinch to zoom", key hints (kbd, .kb, " · Esc") are hidden, and below 600px the ledger folds to two lines (no bars, no route chips, one scrolling tool row).
- Shareable copy: separate artifact "PostalTrace_Phone" (its <title> is PostalTrace_Phone; keep that when regenerating) (https://claude.ai/artifact/MVR5Uk1AFnDxaFnaYn9vst) = the main index.html with `<script>window.PT_SHARE=true;</script>` after `<head>`. PT_SHARE hides Dev Load and the Dev Mode toggle and keeps DEV off. Republish it whenever the main artifact changes (share/index.html is made from live2/index.html).
- Standalone web build (for GitHub Pages / any static host): scratchpad web/ = live2/index.html + PT_SHARE + manifest.webmanifest + icons + .nojekyll + every asset from the main artifact (assets/w3d_*.js, gaol3d.js, amb/, music/, sfx/) + gaol.html. With no Claude account the game saves to localStorage on its own. Delivered as PostalTrace_Phone_web.zip. Target repo: jamtcg04-art/PostalTrace_Phone (public, Pages from main /). PostalTrace_Phone artifact v3 now also carries the sounds, music and gaol.html (v1–v2 were missing them).
