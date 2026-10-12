# Sauna Design — Resume Context

Saved 2026-10-12 so Claude Code can be updated and the session resumed cleanly.
When you reopen this project, re-prime Claude with: "read CONTEXT.md and continue
in auto mode." The user is `gmwingard@gmail.com`.

## The ask (verbatim intent)

Design a backyard sauna in Autodesk Fusion following **Löyly sauna principles**
(user wrote "lolly" — this is the Finnish Löyly aesthetic popularized by Avanto
Architects' Helsinki sauna). Must:

1. Fit 4–5 people comfortably seated.
2. Model the **interior wood (clear cedar)** and the **exterior siding wood
   (lesser-grade cedar)** as **two distinct Fusion components** so the BoM
   splits them cleanly for lumber pricing.
3. End state: fully designed in Fusion + capable of outputting a BoM.
4. Work in **auto mode** — proceed autonomously, no step-by-step approvals.

## Active Fusion document

- Name: `shed_sauna_floorplan v5`
- Hub project: `Backyard Buildings` (project id `a.YnVzaW5lc3M6Z21haWw0NzY3OTA0IzIwMjUxMjAxMTAyMTY3OTAzMQ`)
- Lineage urn: `urn:adsk.wipprod:dm.lineage:li1-pcgLQoW3LVn9zVYnzQ`
- State when work paused: **essentially empty** — just default origin planes +
  a couple of thin rail/shed-base bodies. No components created yet.

## Löyly design principles being applied

- Rectangular dark/warm "box" inside (clear cedar lining, kept clean and simple).
- Outer **wooden cloak** of vertical slat/lamella cedar with narrow gaps
  (venetian-blind feel) — provides privacy, breaks up the mass, lets the siding
  age gracefully.
- Benches at two heights for stratified heat (löyly rises hottest to the top bench).
- Sheltered covered stoop/porch area between outer cloak and inner box where
  bathers can cool down briefly.
- Simple shed/mono-pitch roof for the backyard build (not the full faceted
  waterfront roof of the Helsinki original — overkill for 4–5-person backyard).

## Design parameters locked in (auto-mode decisions)

Units: inches internally, feet in prose.

| Parameter | Value | Note |
|---|---|---|
| Hot-room interior | 8'-0" W × 7'-0" D × 7'-0" H | Comfortable for 4–5 adults |
| Changing/cool-down room | 4'-0" W × 7'-0" D × 7'-0" H | Behind the hot room, same footprint width mirrored |
| Covered stoop depth | 2'-0" | Under extended roof overhang |
| Total exterior footprint | ~13'-6" W × 8'-6" D (framing) | Add 2' stoop = 13'-6" × 10'-6" |
| Wall framing | 2×6 @ 16" o.c. | Fits R-19 batt + foil vapor barrier |
| Roof | Shed, 2:12 pitch, sloping toward the back | Rafters 2×6 @ 16" o.c. |
| Interior finish | 1×6 T&G **clear cedar**, horizontal | Walls + ceiling |
| Exterior cladding | 1×4 **STK/knotty cedar** vertical slats | 3/4" gap between slats, Löyly cloak |
| Floor (hot room) | Removable 2×2 clear cedar duckboard over sloped concrete | |
| Benches | Upper 42" AFF × 24" deep, lower 18" AFF × 20" deep, L-shape | 2×2 clear cedar slats on 2×4 frame |
| Door | 24" × 72" cedar, half-glass, swings outward | Short wall between changing room and hot room; second door changing→outside |
| Window | 18" × 24" fixed, tempered | Opposite wall from stove |
| Stove placeholder | 20" × 20" × 32" tall volume | Modeled only as clearance envelope |

## Component / BoM structure

The Fusion browser should end up with (at minimum):

```
shed_sauna_floorplan v5
├── 01_Framing              (2x material — pricing done separately, not cedar)
├── 02_Interior_ClearCedar  ← ALL clear-cedar bodies live here
│   ├── Wall paneling (1x6 T&G, by wall)
│   ├── Ceiling paneling
│   ├── Bench slats (upper + lower)
│   ├── Bench frames
│   ├── Backrests
│   └── Duckboard floor
├── 03_Exterior_SidingCedar ← ALL knotty/STK cedar bodies live here
│   ├── Vertical lamella slats (by wall)
│   ├── Corner trim
│   ├── Fascia / rake trim
│   └── Door/window casing exterior
├── 04_Openings             (door, window, stove envelope — reference only)
└── 05_Roof_Structure       (roof decking + membrane, non-cedar)
```

**Critical BoM requirement:** the two cedar grades must be separate top-level
components so their rolled-up quantities (count, length, board-feet) can be
exported independently. In Fusion, right-click component → "Create Drawing /
Export BoM" or Data Panel → Bill of Materials.

## Task list (in Claude's TaskList)

1. Define sauna dimensions & parameters — set user parameters in Fusion
2. Build structural framing shell (2×6 skeleton)
3. Create interior clear-cedar component
4. Create exterior cedar siding/lamella component
5. Add door, window, stove placeholder
6. Add roof structure
7. Generate BoM output

All currently **pending**. None started.

## How to execute in Fusion

Use the `mcp__fusion__fusion_mcp_execute` tool with `featureType: "script"` to
run Python scripts against the Fusion API. The `readOnly: false` flag is
required for anything that mutates the design. Keep each script small and
idempotent — build one component at a time, verify with a screenshot via
`mcp__fusion__fusion_mcp_read` queryType=screenshot, then move on.

Useful read queries:
- `queryType: "document", operation: "open"` → list open docs, confirm active
- `queryType: "apiDocumentation"` with searchPattern → find Fusion API classes
- `queryType: "screenshot", direction: "iso-top-right"` → visual check

## Next step when resuming

Start Task #1: write a Python script that creates named user parameters in the
active document for every dimension in the table above, then run it via
`fusion_mcp_execute`. That makes every subsequent script parametric. Then move
through tasks 2–7 in order, screenshotting after each.

## Sources

- [Löyly Sauna — Danish Architecture Center](https://dac.dk/en/magazine/places/loyly-sauna-632)
- [Löyly Sauna by Avanto Architects — Parametric Architecture](https://parametric-architecture.com/loyly-sauna-by-avanto-architects-helsinki-finland/)
- [Trumpkin's Notes On Building A Sauna](https://localmile.org/trumpkins-notes-on-building-a-sauna/) — reference for 2×6 framing + T&G cedar interior conventions
