---
name: wonderworks-media
description: Guides generation of images, video, music, sound effects and storyboards through the WonderWorks MCP tools. Use when the user asks to generate, create, make or render an image, picture, photo, illustration, concept art, video, clip, animation, storyboard, shot list, music, score, soundtrack, sound effect, Foley or ambience — or to animate a still, reuse a generated asset, or check whether an asset has finished rendering.
---

# WonderWorks media generation

Produce media with the `wonderworks` MCP tools. Pick the right tool, write a
prompt with enough specificity to be reproducible, and chain assets by id.

## Choosing the tool

| Request | Tool |
|---|---|
| One still image | `generate-image` |
| A sequence of connected shots, a story, a scene-by-scene visualization, the basis for a longer video | `generate-storyboard` |
| A 5s or 10s clip, or animating an existing still | `generate-video` |
| Score, soundtrack, theme, background music | `generate-music` |
| Single effect, Foley, ambience, impact, transition | `generate-sound` |
| Status or URL of an existing asset | `get-asset` |
| Show an asset created in an earlier turn | `render-asset-widget` |

Ask for more than one shot before reaching for `generate-image` in a loop —
`generate-storyboard` maintains character, location and style continuity across
shots, which repeated single-image calls do not.

## Writing prompts

Generation quality tracks prompt specificity. Expand a thin request into a full
description rather than passing it through verbatim; state the assumptions added
so the user can correct them.

**Image** — subject, composition and framing, pose or action, camera (lens,
angle, distance), setting, lighting, visual style, and explicit exclusions.

**Video** — motion over time first: what moves, how the camera moves, pacing,
scene changes. Then environment, lighting, style. With a `source` first frame,
describe how that exact scene animates rather than re-describing it.

**Music** — genre, mood, instrumentation, tempo, rhythm, structure, production
style, intended use, and whether vocals are wanted.

**Sound** — the source event, environment, acoustic character, intensity,
timing, texture, distance and perspective.

Match `aspect` to the destination: `16:9` for landscape and film, `9:16` for
phone-first and social verticals, `1:1` for square posts and thumbnails.

## Chaining assets

Every generate call returns an asset id. Use ids, not re-uploaded files:

- Keep a character or product consistent across images: pass earlier image ids
  in `sources` (up to 4).
- Animate a still: pass its id as `source` on `generate-video`. `aspect` is then
  ignored — the source frame sets it.
- Build a longer piece: `generate-storyboard`, then `generate-video` per key
  frame using each frame's id as `source`.
- Score a clip: generate the video, then `generate-music` with a duration that
  matches, describing the intended cut.

## Displaying results

The generate tools display their own media player. Never call
`render-asset-widget` in the same turn as a generate call — it renders a second
player for the same asset. Use it only for an asset from an earlier turn or one
the user names by id.

Assets return before rendering finishes; the player fills in when ready. Call
`get-asset` only when a status or URL is actually needed — for example to feed a
finished image into another tool — not to poll on the user's behalf.
