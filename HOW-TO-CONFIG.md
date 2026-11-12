# Flow Canvas — configuration

Every property, tab by tab, then the microflow patterns. Studio Pro hides what a choice makes unused and checks the
rest (see *Studio Pro checks* at the end).

Keys are plain strings your app chooses (`manager`, `HR`, `review`). Every key comparison ignores case. A list of keys
(Variants, Badges, Returns to, Path) is separated by commas.

## Steps tab

| Property | Type | What it is |
|---|---|---|
| Steps | data source | One object per step, **sorted by Order** |
| Step key | expression (String) | Unique within the flow |
| Title, Title (second language) | text | The second title is shown under the first (e.g. Arabic under English) |
| Kind key | expression | Looked up in the Kind map (shape, icon, caption); `start` / `end` work without a map |
| Lane key | expression | The swimlane; empty = no lane |
| Parallel group key | expression | Steps with the same group key (and the same Order) run in parallel |
| Order | expression (Decimal; an Integer attribute works) | The flow's order; ties are a parallel rank |
| Description | text | One line on the card |
| Badges | expression | Badge keys, comma separated, looked up in the Badge map |
| Facts | text | A short line such as `SLA 24 h · Line manager` |
| Condition | text | Your words for when the step runs: the incoming edge is drawn dashed with them, and a skip path goes around |
| Variants | expression | The variants the step runs in; empty = all |
| Returns to | expression | Keys of earlier steps the case can be sent back to (comma separated): a return edge each |
| Return label | text | The words on those return edges |
| Branch key | expression | Inside a parallel group: steps with the same branch key run one after the other (in Order) |
| Locked | Boolean | Cannot be moved, deleted or duplicated (a lock mark is shown) |
| Can move / Can delete / Can duplicate | Boolean, default true | Finer policy, e.g. Can delete = false for a mandatory step |
| Done by several | expression | `'parallel'` or `'sequential'`: a multi-instance marker (a multi-user task, a review panel) |
| Colour key | expression | `blue`, `green`, `amber`, `red`, `purple`, `teal`, `grey`: a coloured top edge |
| Custom content | widgets | Your own card body instead of the widget's |
| Problem | text | Your check's message: red border, listed in Problems |
| Change | expression | Your change key (added / changed / removed, see Keys): a change mark |
| Status | expression | Your status key per step (runtime) |
| Waiting since | text | Shown on the current, returned or error step, two lines at most |
| X, Y | expression (Decimal; Integer works) | Manual layout only |
| Details card | text × 10 | Instructions (two languages), outputs, documents, escalation, notified, effort, help link (http, https or mailto only) and caption, more (one paragraph per line) |

## Edges tab

| Property | What it is |
|---|---|
| Edges from | Expression: `'derived'` (from the steps), `'dataSource'` (your edge objects), `'both'` (derived plus your extra edges, e.g. exceptions) |
| Skip paths | Draw the path around a conditional step (derived edges) |
| Edges, Edge key, From key, To key | Your edge objects |
| Kind key | Your kind key; translated with the Edge kind keys (Maps tab) |
| Label, Variants, Status, Problem | The edge's words, variants, status (the Taken key draws it as part of the path) and your check's message |

**Derived edges**: steps in Order, start → first → … → last → end; a parallel group forks before its steps and joins
after them (each branch a chain when Branch key is used); a step's condition labels its incoming edge; Returns to adds
a return edge.

## Lanes and groups tab

- **Lanes**: Show lanes, Lanes data source, Lane key, Title (two languages), Order, Colour key. A lane key used by a
  step but missing from the data source still gets a lane.
- **Parallel groups**: Groups data source, Group key, Title, Join words (`all must finish`), Problem, Locked (nothing
  dropped into it or moved out, its steps keep their place).

## Returns tab

For flows that keep returns as a list (for example non-persistent objects a microflow builds): Returns data source,
From key, To key, Label. They are added to the steps' Returns to; each return is drawn once.

## Maps tab

- **Kind map**: kind key → shape (task, approval, decision, sub-process, timer, notification, script, manual, start,
  end, custom), icon, caption.
- **Badge map**: badge key → icon, tone (neutral, accent, success, warning, danger), caption.
- **Keys**: your keys for the edge kinds (sequence, conditional, return, exception, timeout), the statuses (pending,
  current, done, skipped, error, returned, taken) and the change marks (added, changed, removed). The widget reads
  your keys and writes your keys (Connect, edge kind).

## Palette tab

Palette source (static items or a data source), Position (start, top, hidden). Static items: kind key, caption, icon,
group. Data source items: data source, kind key, caption, group. A search box appears from 13 items.

## Runtime and variants tab

- **Highlight**: comma-separated step keys to outline (the steps a change touches, your own search result).
- **Current step key**, **Path** (the steps passed, in order, comma separated; a step twice when the case went back;
  `from>to` pairs for edges the order does not tell), **Animate path** (a token travels the path once, and on Replay).
- **Variant** (the variant shown), **Steps of other variants** (`'dim'` or `'hide'`), **Variants for the picker**
  (data source, key, caption). The picker writes the Viewed variant key and runs On variant.

## Actions tab

### Drop context

Attributes of the data view object the widget sits in. A gesture writes its own attributes, then runs its action:

| Attribute | Written by |
|---|---|
| Node key | select, open, move, delete, duplicate |
| Target key | add (the step it comes after), move |
| From key, To key | insert, connect, return, return remove |
| Edge key | insert, edge select / kind / delete, return remove |
| Lane key, Group key | add, insert, move |
| Kind key | add, insert (the palette kind), connect and edge kind (your edge kind key) |
| Position | add, move: the place in the lane (0 = first) |
| Selection | wrap: the selected step keys, comma separated |
| Saved layout | save layout: `key:x,y;key:x,y` |
| Selected step key, Selected edge key | select (and cleared by Escape or a click on the background) |
| Viewed variant key | the variant picker |
| Step action key | a step action of your own (Step menu) |

If an attribute is read-only (the data view is not editable), the gesture is refused before the action runs, and the
canvas says so.

### Actions

| Action | Runs for | Reads |
|---|---|---|
| On step select / open / move / delete / duplicate | the step (step data source) | Node key; Move: Target key, Lane key, Group key, Position |
| On edge select / kind / delete | the edge (edge data source) | Edge key; Kind: Kind key |
| On return | the context | From key, To key: add the return From → To (derived flows) |
| On return remove | the context | From key, To key: remove that return |
| On add | the context | Kind key, Target key, Lane key, Group key, Position |
| On insert | the context | Kind key, From key, To key, Lane key, Group key (bars are translated: see [Inserts around a parallel group](#inserts-around-a-parallel-group)) |
| On connect | the context | From key, To key, Kind key (your return key when drawn back to an earlier step) |
| On wrap / On ungroup | the context | Selection / Group key |
| On save layout | the context | Saved layout |
| On variant | the context | Viewed variant key |
| On jump | the context | From key (the current step), To key: move the running case (also in a read-only canvas) |
| On undo / On redo | the context | nothing: your app keeps the history (Can undo / Can redo grey the buttons out) |
| Step actions (Step menu) | the step | Node key, Step action key; each item: Caption, Icon, Action key, Visible (per step), Offered (always / editable / read-only), Danger |

### Inserts around a parallel group

The fork and join bars are the widget's own; an edge into or out of a bar is translated before your action runs, so
On insert always names real steps:

| Edge dropped on | Group key | From key | To key |
|---|---|---|---|
| step → fork (before the group) | "" | the step | the group's first step |
| fork → step (first in a branch) | the group | "" | the step |
| step → step inside a branch | the group | the step | the next step |
| step → join (last in a branch) | the group | the step | the step after the group ("" at the end) |
| join → step (after the group) | "" | the group's last step | the step |

Start / end markers and bars appear as `start`, `end`, `fork:G` and `join:G` in derived edge keys. A locked group
takes no drop at all.

A missing action switches its gesture off; read-only (an expression) switches all of them off.

### Microflow patterns

```
ACT_Step_Move ($Step, $Context)
  retrieve the target ($Context/TargetKey) in the same flow
  change $Step: LaneKey = $Context/LaneKey, GroupKey = $Context/GroupKey,
                SortOrder = between the target's order and the next step's (the group's order inside a group)
  commit; renumber when the gap gets small; recompute your problems

ACT_Insert ($Context)                       -- an edge was dropped on
  create the step with KindKey = $Context/KindKey, LaneKey, GroupKey
  order = after From key (or first in the group when From key is empty, or the group's order)
  with edges of your own: delete From → To, create From → new and new → To

ACT_AddReturn ($Context)                    -- derived flows
  retrieve the step From key; add To key to its ReturnsTo; commit
```

Refuse in the microflow what your policy forbids even when the page offers it: the canvas shows the change at once
and snaps back by itself when your data does not follow.

### Step menu

Step actions are your own actions on a step: they appear in the step's menu (the ⋯ tool, or A on the keyboard).
Typical ones: *Assign…* (open your assignment dialog), *Run now* (call the automated step's microflow), *Escalate*,
*Skip this step*, *Open the sub-flow*, *Make mandatory*. *Visible* is evaluated per step (for example
`$currentObject/Status = 'current'`), and *Offered* picks the designer (editable), a viewer (read-only) or both. The
action runs for the step, so its microflow takes the step object, and the context for Step action key.

## Side panel tab

Inspector, Details and Preview slots (your widgets, with the selected step's context), Side panel width, Side panel
(auto, shown, hidden). The Details and Problems tabs are built in; your Details slot replaces the built-in card.

## Layout tab

Find (a search box, Ctrl+F) and Full screen (buttons in the toolbar), Flow direction (`'down'`, `'right'`), Spacing (`'compact'`, `'roomy'`), Layout (`'auto'`, `'manual'`), Minimap (auto,
on, off), Export (PNG / SVG), Export file name (expression), Height, Rail below and Narrow below (the widget's width
at which the palette folds to icons, and at which the palette and side panel move above and below the canvas).

## General tab

Read-only (expression), Flow problems (one per line), Direction (auto, left to right, right to left), Colour scheme
(inherit, follow system), Animations, and every interface text, list text and accessibility text. The list texts take
one name per meaning separated by `|`, in a fixed order (Studio Pro counts them).

## Studio Pro checks

- an action whose drop-context attribute is missing (error, named);
- add / insert / move on a flow with lanes but no Lane key, or with parallel groups but no Group key (warning);
- On variant without a Variants data source (warning);
- Edges from `'dataSource'` / `'both'` without an Edges data source or its From / To keys (error);
- a Lanes, Groups, Variants or Returns data source without its keys (error);
- a palette source without items (warning, when add / insert are set);
- duplicate keys in the maps, the edge kind keys or the status keys (error);
- manual layout without X / Y (warning); breakpoints in the wrong order (warning);
- a `|`-list with the wrong number of names (warning).
