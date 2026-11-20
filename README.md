# Verified Vehicle Report: design package (DBIZ2 - Group A)

> DBIZ2 Digital Business Project II, VJCBI College, Foreign Trade University.
> Client: Tasco Corporations. Group A, five members.

**Verified Vehicle Report** is a feature inside the TASCO Mobility app. A car owner creates a vehicle
profile from the VIN and sees one verified record of the car: identification, verified minimum
mileage, maintenance and repair history, battery state of health (only for electric vehicles), and legal
status (inspection, traffic fines, mortgage). Every week the system collects partner-marketplace
listings and shows a market price with a 12-week trend chart to the car owner for reference. When the owner wants to sell, the
system updates the legal status, pre-fills the sale request, and sends the verified vehicle report and
market price to Carpla staff to return more accurate preliminary quote.

This repository holds the **design package only**: requirements, analysis and external design,
written in DBIZ2. There is no code yet. In DBIZ3 the team and its AI coding agent build the product
from these files.

**This README is written for two readers.** People start at section 8 (reading order). AI agents
read sections 1 to 7 before any task and follow them.

---

## 1. Rules for AI agents

1. **The files are the source of truth, not your general knowledge.** If a fact is not in this
   repository, it is not decided. Do not fill the gap with a typical solution.
2. **Every fact has one owning file** (section 3). Read the owner before you use a fact. To change
   a fact, change it in the owner first, then update the files that cite it.
3. **Refer by ID, never retell.** Write `FR-01-03`, `UC-1.05`, `S-F01-02`, `ADR-004`; do not copy
   the sentence. A retold sentence drifts from its source.
4. **Do not invent IDs.** A new item takes the next free number in its owning file (section 4).
   Never reuse or renumber an existing ID, even when the item is removed; mark it removed instead.
5. **Stay inside scope.** Build or design only what W1 lists. Everything in W0 Table 3
   (Won't-have: Sale Alert, Maintenance Alert) stays out, even if it seems useful. The priority
   that counts is the **effective priority** in W4, not the initial priority in W1.
6. **When something is missing or unclear, ask; do not guess.** Add a row to the Open questions
   table of the file you are working on, written as `[NEEDS CLARIFICATION: <question>]`, and mark
   whether it blocks the work. A guess written as a decision is the most expensive kind of error.
7. **Screens follow the external design exactly.** Elements, states and navigation come from the
   Screen Spec; colours, fonts, spacing, corners and frame width come only from the design tokens
   in the UI conventions file of the matching actor (section 3). The Element names in section 3 of
   a Screen Spec are the component names; the numbers on `*-numbered.png` match its `#` column.
8. **Do not edit history.** Accepted ADRs are not rewritten: a new ADR supersedes an old one.
   Change log rows, verification log rows and `AI Log.md` rows are only appended.
9. **Do not edit images by hand.** Images in `docs/External Design/Screen/img/` are exports from
   the team's drawing tool. If a screen changes, say which image must be redrawn.
10. **Keep the traceability files in step.** Any change to a use case, a sequence diagram message
    or a context flow must also be reflected in `docs/Traceability/` (section 3).
11. **Report what you changed and what you could not check**, file by file, at the end of the task.

## 2. Repository map

All documents live in `docs/`. Folder and file names contain spaces, so quote or URL-encode paths.

```text
README.md                          this file
docs/
  Project/                         Lessons 1-2   Project Brief.md, AI Log.md
  Context/                         Lesson 3      CVCA.md, Context Diagram.md
  Use Case Diagram/                Lesson 4      Overview Verified Vehicle Report Use Case Diagram.md,
                                                 UC-1.md, UC-2.md, UC-3.md (one per feature)
  Sequence Diagram/                Lesson 4      S-01.md ... S-10.md
  Architecture/                    Lesson 4      Architecture.md
    adr/                                         ADR-001 ... ADR-006 (decision records)
  Traceability/                    Lessons 3-4   list-of-flow-context-diagram.md,
                                                 list-of-use-case.md,
                                                 list-of-message-sequence-diagram.md,
                                                 traceability.md
  Function List/                   Lessons 5-6   w0-scope.md, w1-function-list.md, w2-cost.md,
                                                 w4-priorities.md, w5-handover.md, w6-cash-flow.md
  Activity Diagram/                Lesson 7      Activity Index.md, AD-UC-<n.nn>.md
  External Design/                 Lesson 8      Screen List.md, Screen Transition.md,
                                                 usage-flow-*.md, ui-conventions-*.md
    Screen/                                      screen-spec-S-Fxx-nn.md
      img/                                       screen images (drawn by the team)
```

There are two actors with screens, so two files each for usage flow and UI conventions:

| Actor | Platform | Usage flow | UI conventions |
|---|---|---|---|
| Car Owner | Mobile, TASCO Mobility app | `usage-flow-tasco-verifed-vehicle-report-system.md` | `ui-conventions-car-owner.md` |
| Carpla Staff | Web, Carpla CRM | `usage-flow-carpla-crm.md` | `ui-conventions-carpla-staff.md` |

## 3. Which file owns which fact

| Fact | Owning file | Cited by |
|---|---|---|
| Product name, client, team, roles, milestones, problems, goals | `docs/Project/Project Brief.md` | every file |
| Stakeholders, value flows, stakeholder requirements `StR-nn` | `docs/Context/CVCA.md` | Architecture |
| System boundary and external entities | `docs/Context/Context Diagram.md` | Use case diagrams, AD lanes |
| Context flows `CTX: flow-nn` | `docs/Traceability/list-of-flow-context-diagram.md` | Traceability, SD |
| Actors and use cases (diagrams) | `docs/Use Case Diagram/` | W0, AD, usage flows |
| Use case list with secondary actors and include/extend | `docs/Traceability/list-of-use-case.md` | Use case diagrams, W0 |
| How the system's parts talk for one use case | `docs/Sequence Diagram/S-nn.md` | W1 source trace, AD |
| Messages `S-X.YY` inside sequence diagrams | `docs/Traceability/list-of-message-sequence-diagram.md` | Traceability |
| Context flow to use case to sequence message mapping | `docs/Traceability/traceability.md` | review, tests |
| System requirements `SyR-nn` (from `StR-nn`), functional and physical design | `docs/Architecture/Architecture.md`, section 1 | ADRs, W1 |
| Technical decisions `ADR-nnn` | `docs/Architecture/adr/` | W1, Architecture |
| Use case inventory `UC-n.nn` and its feature | `docs/Function List/w0-scope.md`, Table 1 | AD index, usage flows, screens |
| Features `F-nn` | W0 Table 2 | W1, screen IDs |
| Won't-have (out of scope) | W0 Table 3 | everyone: never build these |
| Data groups, ILF and EIF (what is stored, what is read) | W0 Table 4 | Screen Spec data sources |
| Functions `FR-ff-nn` with the EARS acceptance criterion | `docs/Function List/w1-function-list.md` | AD, Screen Spec, tests |
| Effective priority (Must, Should, Could) | `docs/Function List/w4-priorities.md` | Screen List, build order |
| Cost, scenarios, baseline for DBIZ3 | `w2-cost.md`, `w5-handover.md`, `w6-cash-flow.md` | project decisions |
| Behaviour of one use case, its branches, object states | `docs/Activity Diagram/AD-UC-n.nn.md` | Screen Spec states, tests |
| Which use cases have a diagram, and why not | `docs/Activity Diagram/Activity Index.md` | |
| Order in which an actor uses the use cases | `usage-flow-*.md` in `docs/External Design/` | Screen Transition |
| Screens `S-Fxx-nn` | `docs/External Design/Screen List.md` | everything about screens |
| Navigation between screens | `docs/External Design/Screen Transition.md` | Screen Spec section 5 |
| Language, formats, components, messages, design tokens | `ui-conventions-car-owner.md`, `ui-conventions-carpla-staff.md` | every screen of that actor |
| One screen: elements, states, interactions, rules | `docs/External Design/Screen/screen-spec-S-Fxx-nn.md` | build and test |
| Mistakes AI made and what the team decided | `docs/Project/AI Log.md` | |

## 4. IDs

| ID | Pattern | Example | Defined in |
|---|---|---|---|
| Stakeholder requirement | `StR-nn` | StR-01 | CVCA, Architecture section 1 |
| System requirement | `SyR-nn` | SyR-01 | Architecture section 1 |
| Decision | `ADR-nnn` | ADR-004 | `docs/Architecture/adr/` |
| Context flow | `CTX: flow-nn` | flow-02 | `list-of-flow-context-diagram.md` |
| Use case | `UC-f.nn` (f = feature number) | UC-1.05 | W0 Table 1 |
| Feature | `F-nn` (`F-00` = platform and access) | F-01 | W0 Table 2 |
| Function (L2) | `FR-<feature>-<nn>`, with a suffix for a work item: `-FE` front end, `-BE` back end, `-INT` integration | FR-01-02-INT | W1 |
| Sequence diagram | `S-nn` (file) and `S-X.YY` (message) | S-01, S-1.03 | `docs/Sequence Diagram/` |
| Activity diagram | `AD-UC-<f.nn>` | AD-UC-1.05 | `docs/Activity Diagram/` |
| Screen | `S-F<feature>-<nn>` (`S-F00-nn` = shared) | S-F01-02 | Screen List |

The chain that every change must keep intact:

```text
StR -> SyR -> UC -> F -> FR (EARS) -> AD (cites FR on each system action) -> S (cites UC and FR)

CTX flow -> UC -> SD message      (kept in docs/Traceability/traceability.md)
```

## 5. How to do common tasks

**Before any task:** read this README, `Project Brief.md`, then the owning files of the facts the
task touches (section 3).

**Change or add a function.** Edit W1 (ID, EARS criterion, source trace), then W4 (effective
priority and justification), then W2 and W5 if the cost changes. Then update every AD and Screen
Spec that cites the FR.

**Add a use case.** Add it to the Use Case Diagram of its feature and to `list-of-use-case.md`,
then W0 Table 1 with its feature, then its functions in W1, then the sequence messages and a row
in `traceability.md`, then a row in `Activity Index.md`, then its place in the usage flow.

**Add or change a screen.** Add the row to `Screen List.md`, the node and arrows to
`Screen Transition.md`, the screen to the matching usage-flow step, and the Screen ID to the
"Screens touched" section of each AD it serves. Then write the Screen Spec, using a finished one
(for example `screen-spec-S-F01-04.md`) as the model, and list the images the team must draw.

**Build a screen (DBIZ3).** Read in this order: the Screen Spec, the FRs it lists, the AD whose
actions it covers, the UI conventions file of that actor. Build every state in the Screen Spec.
The acceptance tests are the EARS criteria of the FRs and the branches in the AD.

**Check traceability.** Report five lists, with file and ID, and write "none" when a list is
empty. Do not change files while checking.

- A. FRs with a user-visible effect that no Screen Spec lists.
- B. Screens or elements that serve no FR.
- C. Actor actions in an AD with no Screen ID.
- D. "Goes to" targets in a Screen Spec that are not in `Screen Transition.md`.
- E. Usage-flow steps with no screen, screens in no usage-flow step, usage-flow arrows with no path
  in the transition diagram.

## 6. Notation

- Diagrams are Mermaid code blocks inside the `.md` files. The use case diagrams use
  `usecase-beta` and the activity diagrams use swimlane diagrams, which need a recent Mermaid
  version; if a block does not render, paste it into [mermaid.live](https://mermaid.live).
- Car Owner interface text is in Vietnamese only, addressing the user as "bạn"
  (`ui-conventions-car-owner.md`). Documents are written in English; proper names such as VIN,
  VETC, Carpla Service and SoH stay as they are.
- Dates `dd/MM/yyyy`, times `HH:mm` (24-hour), money in VND with dots for thousands and the suffix
  `₫` (`850.000.000 ₫`), mileage as `45.230 km`.
- Person-hours, rates and thresholds follow the class Parameters of the Lesson 5 workbook. The
  benefit figures in W6 are team estimates for teaching.

## 7. State of this package

An agent must not treat the gaps below as decided. The team should update this list before each
milestone.

- External systems the product reads (VETC, Carpla Service, the electronic invoice portal, EV
  charging stations, the inspection, traffic fine and secured transaction portals, partner
  marketplaces) are read-only data sources (W0 Table 4). Do not design writes to them.
- `docs/Activity Diagram/` has 16 diagrams (`AD-UC-1.01`, `1.03`, `1.05`, `1.06`, `1.07`, `2.04`,
  `2.05`, `2.07` to `2.10`, `3.01`, `3.03` to `3.06`). The other use cases of the 30 in W0 Table 1
  are recorded in `Activity Index.md` with the reason they have no diagram.
- Screen Specs exist for `S-F01-01`, `S-F01-02`, `S-F01-03`, `S-F01-04`, `S-F01-06`, `S-F02-01`,
  `S-F03-01` and `S-F03-03`. Other screens in `Screen List.md` have no Screen Spec yet.
- Some Screen Spec file names do not follow the pattern `screen-spec-S-Fxx-nn.md`
  (`screen spec S-F01-01.md`, `screen spec S-F01-02.md`, `screen spec S-F01-03.md`,
  `screen spec-S-F03-03.md`). Rename them in one commit, then fix links.
- Open questions marked `[NEEDS CLARIFICATION: ...]` are listed in the AD and Screen Spec files;
  those with an answer show it in the last column.

## 8. Reading order for people

| Step | Lesson | File | What it shows |
|---|---|---|---|
| 1 | L1-L2 | [Project Brief.md](docs/Project/Project%20Brief.md) | Client, problems, goals, team roles, milestones |
| 2 | L3 | [CVCA.md](docs/Context/CVCA.md) | Stakeholders, value flows, problems, stakeholder requirements |
| 3 | L3 | [Context Diagram.md](docs/Context/Context%20Diagram.md), [list-of-flow-context-diagram.md](docs/Traceability/list-of-flow-context-diagram.md) | The system as a black box, the entities around it, every flow |
| 4 | L4 | [Overview Use Case Diagram](docs/Use%20Case%20Diagram/Overview%20Verified%20Vehicle%20Report%20System%20Use%20Case%20Diagram.md), [UC-1](docs/Use%20Case%20Diagram/UC-1.md), [UC-2](docs/Use%20Case%20Diagram/UC-2.md), [UC-3](docs/Use%20Case%20Diagram/UC-3.md), [list-of-use-case.md](docs/Traceability/list-of-use-case.md) | Who wants the system to do what, per feature |
| 5 | L4 | [Sequence Diagram folder](docs/Sequence%20Diagram/), [list-of-message-sequence-diagram.md](docs/Traceability/list-of-message-sequence-diagram.md) | How the parts talk for each use case |
| 6 | L4 | [traceability.md](docs/Traceability/traceability.md) | Context flow, use case and sequence messages in one table |
| 7 | L4 | [Architecture.md](docs/Architecture/Architecture.md), [adr/](docs/Architecture/adr/) | System requirements, design, six decisions |
| 8 | L5 | [w0-scope.md](docs/Function%20List/w0-scope.md) | Use case inventory, features, Won't-have, data functions |
| 9 | L5 | [w1-function-list.md](docs/Function%20List/w1-function-list.md) | Every function with its EARS criterion, split into work items |
| 10 | L5 | [w2-cost.md](docs/Function%20List/w2-cost.md), [w4-priorities.md](docs/Function%20List/w4-priorities.md), [w5-handover.md](docs/Function%20List/w5-handover.md) | Cost, priorities, threshold checks, scenarios |
| 11 | L6 | [w6-cash-flow.md](docs/Function%20List/w6-cash-flow.md) | Cash flow, NPV, IRR, payback |
| 12 | L7 | [Activity Index.md](docs/Activity%20Diagram/Activity%20Index.md), [Activity Diagram folder](docs/Activity%20Diagram/) | Inputs check, then activity diagrams |
| 13 | L8 | [Car owner usage flow](docs/External%20Design/usage-flow-tasco-verifed-vehicle-report-system.md), [Carpla CRM usage flow](docs/External%20Design/usage-flow-carpla-crm.md) | One usage flow per actor |
| 14 | L8 | [Screen List.md](docs/External%20Design/Screen%20List.md), [Screen Transition.md](docs/External%20Design/Screen%20Transition.md) | Screens and how users move between them |
| 15 | L8 | [ui-conventions-car-owner.md](docs/External%20Design/ui-conventions-car-owner.md), [ui-conventions-carpla-staff.md](docs/External%20Design/ui-conventions-carpla-staff.md) | Language, formats, components, design tokens |
| 16 | L8 | [Screen folder](docs/External%20Design/Screen/) | Screen Specs with state images |
| all | all | [AI Log.md](docs/Project/AI%20Log.md) | Where AI suggested something wrong, and what the team did |
