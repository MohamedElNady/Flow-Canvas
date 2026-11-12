# Getting started: a process designer in an afternoon

This builds a small designer for an approval process: steps in lanes, added from a palette, moved, sent back. It
assumes Mendix 10.6 or later and the widget in your app (`widgets/elnady.FlowCanvas.mpk`).

## 1. The domain

```
Process (Name)
Step    (StepKey, Title, KindKey, LaneKey, GroupKey, SortOrder: Decimal, ReturnsTo, Mandatory: Boolean)   Step_Process → Process
Lane    (LaneKey, Title, SortOrder)                                                                  Lane_Process → Process
FlowContext  non-persistent: NodeKey, TargetKey, FromKey, ToKey, EdgeKey, LaneKey, GroupKey, KindKey,
             Position: Integer, Selection, SelectedNodeKey, SelectedEdgeKey      FlowContext_Process → Process
```

No edge entity: the widget derives the edges from SortOrder, GroupKey and ReturnsTo.

## 2. The page

1. A page with a parameter `Process`, or a data source microflow `DS_FlowContext` that creates a `FlowContext` for it.
2. A **data view** of the `FlowContext` (editable).
3. **Flow Canvas** inside it.

## 3. Show the flow

| Tab | Property | Value |
|---|---|---|
| Steps | Steps | Database, `Step` where `[Step_Process = $FlowContext/FlowContext_Process]`, sort by `SortOrder` |
| | Step key / Title / Kind key / Lane key / Parallel group key / Order | the attributes |
| | Returns to | `$currentObject/ReturnsTo` |
| | Can delete | `not($currentObject/Mandatory)` |
| Lanes and groups | Lanes | Database, `Lane` of the process, sort by `SortOrder`; Lane key, Title, Order |
| Maps | Kind map | `task` → Task, `approval` → Approval, `notify` → Notification … |

Run the app: the steps are drawn top to bottom in their lanes, with start and end markers.

## 4. Let people change it

| Tab | Property | Value |
|---|---|---|
| Palette | Static items | `task` Task, `approval` Approval, `notify` Notification |
| Actions › Drop context | every attribute | the `FlowContext` attribute of the same name |
| Actions › Step actions | On move | `ACT_Step_Move(Step = $currentObject, Context = $dataView)` |
| | On step delete | `ACT_Step_Delete(Step, Context)` |
| Actions › Flow actions | On add, On insert | `ACT_Step_Add(Context)`, `ACT_Step_Insert(Context)` |
| Actions › Edge actions | On return, On return remove | `ACT_Return_Add(Context)`, `ACT_Return_Remove(Context)` |

The microflows (each ends with a commit, so the canvas reloads):

- **ACT_Step_Add**: create a `Step` with `KindKey = $Context/KindKey`, `LaneKey = $Context/LaneKey`, `GroupKey =
  $Context/GroupKey`, `SortOrder` just after the step `$Context/TargetKey` (or first when it is empty), a new key.
- **ACT_Step_Insert**: the same, after `$Context/FromKey`. An insert on the first edge of a parallel branch has an
  empty From key and the group's key: give the new step the group's order.
- **ACT_Step_Move**: set `LaneKey`, `GroupKey` and a `SortOrder` after the target.
- **ACT_Step_Delete**: refuse a mandatory step (the canvas hides Delete for it, the microflow is the rule), delete,
  clear the `ReturnsTo` that pointed at it.
- **ACT_Return_Add**: on the step `$Context/FromKey`, add `$Context/ToKey` to `ReturnsTo` (comma separated).
- **ACT_Return_Remove**: remove `$Context/ToKey` from that step's `ReturnsTo`.

Studio Pro names any drop-context attribute an action still needs.

## 5. Try it

Drag *Approval* onto an edge: it is inserted between the two steps. Drag a step into another lane. Select a step,
drag its handle to an earlier step: a return. Tab into the canvas and use the arrow keys, Space to pick a step up,
Enter to drop it. Then read [BUILD-A-FLOW.md](BUILD-A-FLOW.md) for everything people can do on the canvas, and
[../HOW-TO-CONFIG.md](../HOW-TO-CONFIG.md) for edges of your own, variants, cases, the side panel and the texts.
