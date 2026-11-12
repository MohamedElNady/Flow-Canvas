# Flow Canvas — how to test

Three ways to try Flow Canvas, from the quickest to the most complete, and the checks to run before you ship a
designer or a case viewer built on it.

## 1. Try the online demo

Open <https://aiwidgets-sandbox.mxapps.io/p/flow-designer> (menu **Flow Canvas**). The purchase request is a draft
version with lanes, a parallel review, a conditional CFO step and a problem to fix; **Case viewer** shows four running
cases. **Reset demo data** brings everything back to its starting state.

## 2. Import the demo module into your app

`FlowCanvasDemo.mpk` (in this repository and on the [release page](https://github.com/MohamedElNady/Flow-Canvas/releases/latest))
is the complete demo as a Mendix module, with the widget package inside it.

1. Studio Pro **10.24.10 or later** (module packages import into the same Studio Pro version or a newer one; the
   widget itself works from Mendix 10.6).
2. **File → Import module package…** and pick `FlowCanvasDemo.mpk`.
3. Add `FlowCanvasDemo.Flows_Web` to your navigation. The module uses Atlas Core (layout `Atlas_TopBar`) and Community
   Commons (`StringSplit`, for the saved layout).
4. Run the app, open the flows list and press **Reset demo data** on the designer to seed it.

## 3. Check it by hand

| Try | Expect |
|---|---|
| Drag *Approval* from the palette onto the edge between Submit and Manager approval | A step between them, in the Manager lane, saved |
| Drag Notify requester to the Finance lane | It moves there and stays after a reload |
| Select Manager approval (Mandatory) | No Delete in its tools; Delete key does nothing |
| Select Notify requester, drag its handle to Submit request | A dashed return edge, labelled *Sent back* |
| Click that return's label, Delete | The return is gone |
| Shift-click two steps, *Run in parallel* | They become a parallel group with fork and join bars |
| Tab into the canvas, arrows, Space, arrows, Enter | The step moves, announced |
| Variant = Emergency | Legal review dims |
| Open version 1 | Read-only: no palette, no tools |
| Case viewer, C-1001, *Next step* | The current step moves on, the path grows |

## Tested Mendix versions

Flow Canvas 1.0.0 was checked on Mendix 10.24.10 (the full demo, 21 browser checks), 11.15.0 (the demo app converted:
the same checks, plus a smoke module) and 10.6.11 (a smoke module placing the widget with every data source, action
and context attribute, Integer attributes on the Decimal expressions). See the README's compatibility table.
