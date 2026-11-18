# Playable ad demos

Three playable ads, each a single HTML file under 40 KB with no engine, no libraries and no image or audio assets. All graphics are drawn in code and all sound is synthesized with WebAudio at runtime.

Play them: https://kunalbehbud.github.io/playable-ads/

| Demo | Genre | File |
| --- | --- | --- |
| **Hex Bloom 3D** | 3D hex-stack puzzle (WebGL) | `hex-bloom/index.html` |
| **Froggo Hunt** | Hidden object | `froggo-hunt/index.html` |
| **Jelly Tubes** | Color sort puzzle | `jelly-tubes/index.html` |

## Hex Bloom 3D

Drag stacks of hex tiles onto the board. Neighbouring stacks with the same top colour pour into each other, and a run of seven pops.

- **Raw WebGL, no engine:** a small renderer written for this ad. Geometry for every tile is batched into one dynamic vertex buffer per frame, lit per vertex, with bevelled edges and a darkening gradient on the sides that reads as ambient occlusion between stacked tiles.
- **Own 3D math:** projection and view matrices, world-to-screen projection for UI effects, and ray-plane picking for drag and drop are written out by hand.
- **Auto-framing camera:** the camera distance is solved at runtime so the board and tray fit between the HUD and the CTA on any aspect ratio.
- **Shadows:** projected hex shadows that stretch with stack height and soften as a dragged stack lifts.
- **Merge rules as an animation queue:** transfers and clears are resolved step by step, so chain reactions play out as a sequence of flips with rising pitch.
- **Game feel:** tiles flip over a horizontal axis as they travel, the dragged stack bends because upper tiles lag behind, haptics on place and clear.
- **Tutorial hand:** scores every stack and cell combination and demonstrates the move with the biggest merge.

## Froggo Hunt

Find the frogs hiding in an illustrated village scene before the timer runs out.

- **Scene drawn in code:** the whole illustration (houses, trees, pond, fence, clutter) is vector-drawn once into an offscreen canvas, so the scene costs one `drawImage` per frame.
- **Camera:** drag to pan, pinch or mouse wheel to zoom, cinematic intro sweep, automatic fly-to on hints.
- **Camouflage:** each frog takes the colour of the prop it hides in (bush, rock, lily pad, roof, cloud, tree, window, laundry) and wakes up when found.
- **Flow:** intro sweep → guided first find → timed search → end card with CTA. Losing shows how many frogs are still hiding.
- **Help that adapts:** an idle hint, a hint after three misses in a row, and a hint button. The timer waits for the first touch.

## Jelly Tubes

Sort the jellies so every tube holds one colour.

- **Flow:** guided tutorial level → real level → end card with CTA.
- **Tutorial hand driven by a solver:** a BFS solver computes the next best move from the current board, so the hint is correct even after the player goes off-script.
- **Game feel:** squash and stretch, bounce landings, eye tracking and blinking, rising-pitch combo sounds.

## Shared

- **Ad network hooks:** MRAID (`mraid.open`, `ready`, `viewableChange`), Meta (`FbPlayableAd.onCTAClick`), Google (`ExitApi.exit`), ironSource DAPI, Mintegral (`gameReady`, `gameEnd`, `install`). Audio starts only after the first tap and pauses when the ad is not viewable.
- **A/B variants:** `?v=a` and `?v=b` change difficulty, timing, palette and CTA copy from one build.
- **Funnel events:** every step from `ready` and `first_interaction` to `end_card` and `cta` is recorded with a timestamp and variant, posted to the parent frame and kept in `window.__playable.events`.
- **Responsive:** portrait and landscape, device-pixel-ratio aware.
- **Debug:** `?debug=1` shows FPS and a live event log.
