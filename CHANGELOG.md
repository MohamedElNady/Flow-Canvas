# Changelog

All notable changes to this widget are documented here (format: Keep a Changelog, versions: SemVer).

## [Unreleased]

### Added
- M0: specification, demo plan, decisions, build setup, layout-engine size probe.
- M1: the drawing (lanes, parallel groups with fork / join bars, conditional and skip edges, curved return edges,
  start / end markers, badges, facts, status, waiting since, problems, change and lock marks), our own layered
  layout, zoom / pan / fit, minimap, details card, problems tab, variant picker, dark and right to left.
- M2: palette (beside, rail, above; search from 13 items), drag a kind onto an edge (insert) or into a lane / group /
  after a step (add), move a step, connect two steps (handle or C), the edge menu (kind, delete), delete, duplicate,
  run in parallel / ungroup, pending changes that snap back after 4 s, keyboard navigation along the flow, keyboard
  move and connect, announcements, the path token (Replay), PNG / SVG export, manual layout with Save layout and
  Tidy up.
- M3 (review with a first adopting team): several returns per step (Returns to with comma keys) and a Returns data source;
  On return / On return remove for derived flows (drag back, or R + arrows + Enter); Branch key (sequential branches
  inside a parallel group); Can move / Can delete / Can duplicate and a group Locked flag; Edges from 'both'
  (derived plus the app's extra edges); bars translated before On insert; a read-only drop context
  refuses the gesture and says so; the app's edge kind keys everywhere; Taken key; Side panel (auto / shown /
  hidden); Export file name; E walks a step's edges, G during a keyboard move joins / leaves a group, Shift+Enter in
  the palette adds after a group, Enter adds on the selected edge; Escape and a background click clear the app's
  selection; screen-reader descriptions per step and announcements for every change; a case path through returns and
  skips, and from>to pairs; return tracks of one step apart, a halo where a return crosses an edge; badges wrap to a
  second row, the waiting text to two lines; reduced motion; a stronger focus ring; a smaller minimap; Studio Pro
  checks for the return actions, the group key, the Returns keys and the |-lists.
- Step menu (the app's own step actions with a per-step Visible and a designer / viewer / both mode), On jump (move a
  running case, also read-only: drag the current step's handle or J), multi-instance marker (Done by several), Find
  (Ctrl+F), Highlight keys, Full screen, Undo / Redo (On undo / On redo, Can undo / Can redo, Ctrl+Z / Ctrl+Y).
- A returning step is placed at the outer end of its cell (no crossing inside one lane).