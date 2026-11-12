# Building a flow on the canvas

What the people who use your designer can do, with the mouse and with the keyboard. What they are offered depends on
the actions your app sets (a missing action switches its gesture off) and on the step's flags (Locked, Can move, Can
delete, Can duplicate) and its group's lock.

## Add steps

- **Drag a kind from the palette** into a lane: it lands after the step before the pointer, in that lane (a bar shows
  where). Into a parallel group: it joins the group.
- **Drop it on an edge**: it is inserted between the two steps (the edge turns blue under the pointer). On the edge
  into a parallel group it comes before the group; on a branch's edges it joins the group.
- **Click or press Enter on a palette item**: added on the selected edge, or after the selected step (or at the end).
  After a step of a parallel group it joins the group; **Shift+Enter** adds it after the group.

## Move and arrange

- **Drag a step** to another place or lane. Into a parallel group it joins it; out of it, it leaves.
- **Keyboard**: Space picks the focused step up, the arrow keys move it along the flow and across lanes, **G** joins
  the parallel group beside it (or leaves its group), Enter or Space drops, Escape cancels.
- **Run in parallel**: Shift-click (or Shift+arrows) two or more steps, then *Run in parallel* (or **G**). **Ungroup**
  (or **U**) makes the steps of a group follow each other again.
- **Manual layout** (when your app keeps positions): drag a step anywhere; *Save layout* keeps all positions, *Tidy
  up* lays the flow out again.

## Connect and send back

- **Connect** (flows with edges of their own): select a step and drag its handle to another step, or press **C**,
  arrows to the target, Enter. Drawn back to an earlier step, the edge is a return.
- **Send back** (derived flows): drag the handle to an earlier step, or press **R**, arrows to the earlier step, Enter.
  The step can now send the case back there.
- **Edges**: click an edge or its label for its menu: the kind (sequence, conditional, return, exception, timeout) and
  Delete. A return of a derived flow has Delete only. **E** walks the focused step's edges; Enter selects, Delete
  removes.

## Delete, duplicate

Select a step: its tools are above it (connect, run in parallel, ungroup, duplicate, delete), or press **D** to
duplicate and **Delete** to remove. A locked or mandatory step offers neither.

## Your own actions, and a running case

- **Step menu**: select a step and open ⋯ (or press **A**): the actions your app offers on it (Assign, Run now,
  Escalate, Skip, Open the sub-flow…). Arrows and Enter run one.
- **Move the case** (a case viewer with On jump): select the current step and drag its green handle to another
  step, or press **J**, arrows to the step, Enter.
- **Undo / Redo**: the toolbar buttons, Ctrl+Z and Ctrl+Y (when your app keeps a history).

## See and check

- **Zoom**: Ctrl + wheel, + / −, **0** fits the whole flow; the minimap (**M**) shows where you are on a big flow.
- **Variants**: the picker dims (or hides) the steps of the other variants.
- **Problems**: your app's checks; a click on one selects its step.
- **Details**: the selected step's instructions, documents, escalation, help.
- **Export**: PNG or SVG of the whole flow.
- **Find**: Ctrl+F (or the search button), type part of a name: matching steps are outlined, Enter goes to the next.
- **Full screen**: the button at the end of the toolbar.

## What happens after a change

The change is shown at once and the step is marked as waiting. Your app saves it and the canvas redraws from your
data. If the app does not follow within 4 seconds (it refused, or failed), the change snaps back and a screen reader
hears that it was undone. If the page is read-only, the change is not made and the canvas says so.

## Keyboard summary

| Key | Does |
|---|---|
| Arrows | along the flow (down / up, or across in a left-to-right flow), and to the step beside |
| Home / End | the first / last step |
| Enter | select and open the step |
| Space | pick up / drop (move) |
| G | while moving: join or leave a parallel group; otherwise run the selection in parallel |
| U | ungroup |
| C | connect from the focused step |
| R | send back from the focused step |
| E | walk the focused step's edges |
| D | duplicate |
| Delete | delete the step (or the focused edge) |
| + / − / 0 | zoom in / out / fit |
| M | minimap |
| A | the step menu (your actions) |
| J | move a running case from the current step |
| Ctrl+F | find a step |
| Ctrl+Z / Ctrl+Y | undo / redo |
| Escape | cancel; clear the selection |
