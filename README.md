# Verified Vehicle Report: design package (DBIZ2 - Group A)

> DBIZ2 Digital Business Project II, VJCBI College, Foreign Trade University.
> Client: Tasco Corporations. Group A, five members.

**Verified Vehicle Report** is a feature inside the TASCO Mobility app. A car owner creates a vehicle
profile from the VIN and sees one verified record of the car: identification, verified minimum
mileage, maintenance and repair history, battery state of health (electric vehicles only) and legal
status (inspection, unpaid traffic fines, mortgage). Every week the system collects listings from
partner marketplaces and shows the owner a market price with a 12-week trend chart, for reference.
When the owner wants to sell, the system re-checks the legal status, pre-fills the sale request and
sends the verified report and the market price to Carpla staff, who return a more accurate
preliminary quote.

This repository holds the **design package only**: requirements, analysis and external design,
written in DBIZ2. There is no code yet. In DBIZ3 the team and its AI coding agent build the product
from these files.

**This README is written for two readers.** People can start at section 8 (reading order). AI agents
read sections 1 to 7 before any task and follow them. Abbreviations and project terms are explained
in [Glossary.md]([https://github.com/giangnhk63db/DBIZ2-GROUPA-Verified-Vehicle-Report/blob/ec0588654a9926e8201e59026b5d7a39e8df804d/Docs/Project/Glossary.md]); read it once, and look terms up there instead of guessing.

## At a glance

| | |
|---|---|
| Product | Verified Vehicle Report, inside the TASCO Mobility app |
| Client | Tasco Corporations (Carpla Sale Department) |
| Actors with screens | Car Owner (mobile, TASCO Mobility app); Carpla Staff (web, Carpla CRM) |
| Features | 3: F-01 Vehicle Passport, F-02 Market Price, F-03 Sell Request & Preliminary Quote |
| Size of the design | 30 use cases, 10 sequence diagrams, 6 ADRs, 31 functions (L2) split into 49 work items (L3), 16 activity diagrams, 11 screens, 10 Screen Specs |
| Estimate (W2, W5) | Must only 168.4 person-hours; Must + Should 216.8 (the figure given to the client); full scope 293.2 |
| Language | Documents in English; Car Owner interface text in Vietnamese |
| Status | Design package for hand-over to DBIZ3. Gaps are listed in section 7 |

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
   in the UI conventions file of the matching actor (section 3). The Element names in the element
   inventory of a Screen Spec are the component names; the numbers on `*-numbered.png` match its
   `#` column.
8. **Use the project's words.** Terms and abbreviations are defined in [Glossary.md](Glossary.md).
   Do not introduce a synonym for a defined term. When a new term or abbreviation enters any file,
   add it to the glossary in the same change.
9. **Do not edit history.** Accepted ADRs are not rewritten: a new ADR supersedes an old one.
   Change log rows, verification log rows and `AI Log.md` rows are only appended.
10. **Do not edit images by hand.** Images in `Docs/External Design/Screen/img/` are exports from
    the team's drawing tool. If a screen changes, say which image must be redrawn.
11. **Keep the traceability files in step.** Any change to a use case, a sequence diagram message
    or a context flow must also be reflected in `Docs/Traceability/` (section 3).
12. **Do not fix the gaps of section 7 on your own.** Work around them, record them as open
    questions, and report them.
13. **Report what you changed and what you could not check**, file by file, at the end of the task.

## 2. Repository map

All documents live in `Docs/` (capital D). Folder and file names contain spaces, so quote or
URL-encode paths. The lesson number is the DBIZ2 lesson in which the folder was produced.

```text
README.md                          this file
Glossary.md                        abbreviations and project terms
Docs/
  Project/                         Lessons 1-2   Project Brief.md, AI Log.md
  Context/                         Lesson 3      CVCA.md, Context Diagram.md
  Use Case Diagram/                Lesson 4      Overview Verified Vehicle Report System Use Case Diagram.md,
                                                 UC-1.md, UC-2.md, UC-3.md (one per feature)
  Sequence Diagram/                Lesson 4      S-01.md ... S-10.md
  Architecture/                    Lesson 4      Architecture.md
    adr/                                         ADR-001 ... ADR-006 (decision records), readme
  Traceability/                    Lessons 3-4   list-of-flow-context-diagram.md,
                                                 list-of-use-case.md,
                                                 list-of-message-sequence-diagram.md,
                                                 traceability.md
  Function List/                   Lessons 5-6   w0-scope.md, w1-function-list.md, w2-cost.md,
                                                 w4-priorities.md, w5-handover.md, w6-cash-flow.md
  Activity Diagram/                Lesson 7      Activity Index.md, AD-UC-<n.nn>.md
  External Design/                 Lesson 8      Screen List.md, Screen Transition.md,
                                                 Usage-flow-*.md, Ui-conventions-*.md
    Screen/                                      one Screen Spec per screen (see section 7 for names)
      img/                                       state images drawn by the team
        img-numbered/                            the same images with numbered elements
```

Two actors have screens, so there are two usage-flow files and two UI-conventions files:

| Actor | Platform | Usage flow | UI conventions |
|---|---|---|---|
| Car Owner | Mobile, TASCO Mobility app | `Usage-flow-tasco-verifed-vehicle-report-system.md` | `Ui-conventions-car-owner.md` |
| Carpla Staff | Web, Carpla CRM | `Usage-flow-carpla-crm.md` | `Ui-conventions-carpla-staff.md` |

(`verifed` is spelled that way in the real file name. Keep it until the file is renamed in one commit.)

## 3. Which file owns which fact

| Fact | Owning file | Cited by |
|---|---|---|
| Product name, client, team, roles, milestones, problems, goals | `Docs/Project/Project Brief.md` | every file |
| Abbreviations and project terms | `Glossary.md` | every file |
| Stakeholders, value flows, problems `P-n`, stakeholder requirements `StR-nn` | `Docs/Context/CVCA.md` | Architecture |
| System boundary and external entities | `Docs/Context/Context Diagram.md` | Use case diagrams, AD lanes |
| List of all context flows `CTX: flow-nn` | `Docs/Traceability/list-of-flow-context-diagram.md` | Traceability, sequence diagrams |
| Actors and use cases (diagrams) | `Docs/Use Case Diagram/` | W0, AD, usage flows |
| List of all use cases `UC-f.nn` in the diagrams | `Docs/Traceability/list-of-use-case.md` | Use case diagrams, W0 |
| How the system's parts talk for one use case | `Docs/Sequence Diagram/S-nn.md` | W1 source trace, AD |
| List of all messages `S-X.YY` in the sequence diagrams | `Docs/Traceability/list-of-message-sequence-diagram.md` | Traceability |
| Mapping context flow to use case to sequence message | `Docs/Traceability/traceability.md` | review, tests |
| System requirements `SyR-nn` (from `StR-nn`), functional and physical design | `Docs/Architecture/Architecture.md`, sections 1 to 3 | ADRs, W1 |
| Technical decisions `ADR-nnn` | `Docs/Architecture/adr/` | W1, Architecture |
| Use case inventory `UC-f.nn` and its feature | `Docs/Function List/w0-scope.md`, Table 1 | AD index, usage flows, screens |
| Features `F-nn` | W0 Table 2 | W1, screen IDs |
| Won't-have (out of scope) | W0 Table 3 | everyone: never build these |
| Data groups, ILF and EIF (what is stored, what is read) | W0 Table 4 | Screen Spec data sources |
| Functions `FR-ff-nn` with the EARS acceptance criterion, and work items `FR-ff-nn-FE/BE/INT` | `Docs/Function List/w1-function-list.md` | AD, Screen Spec, tests |
| Effective priority (Must, Should, Could) | `Docs/Function List/w4-priorities.md` | Screen List, build order |
| Cost, scenarios, baseline for DBIZ3 | `w2-cost.md`, `w5-handover.md`, `w6-cash-flow.md` | project decisions |
| Behaviour of one use case, its branches, object states, business rules `BR-<topic>-nn` | `Docs/Activity Diagram/AD-UC-<f.nn>.md` | Screen Spec states and rules, tests |
| Which use cases have a diagram, and why not; object state lists | `Docs/Activity Diagram/Activity Index.md` | data model in DBIZ3 |
| Order in which an actor uses the use cases | `Usage-flow-*.md` in `Docs/External Design/` | Screen Transition |
| Screens `S-Fxx-nn` | `Docs/External Design/Screen List.md` | everything about screens |
| Navigation between screens | `Docs/External Design/Screen Transition.md` | Screen Spec, navigation section |
| Language, formats, components, messages, design tokens | `Ui-conventions-car-owner.md`, `Ui-conventions-carpla-staff.md` | every screen of that actor |
| One screen: elements, states, interactions, screen rules `SR-nnn` | `Docs/External Design/Screen/` | build and test |
| Mistakes AI made and what the team decided | `Docs/Project/AI Log.md` | |

## 4. IDs

| ID | Pattern | Example | Defined in |
|---|---|---|---|
| Problem | `P-n` | P-3 | CVCA section 3 |
| Stakeholder requirement | `StR-nn` | StR-01 | CVCA section 4, Architecture section 1 |
| System requirement | `SyR-nn` | SyR-01 | Architecture section 1 |
| Decision | `ADR-nnn` | ADR-004 | `Docs/Architecture/adr/` |
| Context flow | `CTX: flow-nn` | CTX: flow-02 | `list-of-flow-context-diagram.md` |
| Use case | `UC-f.nn` (f = feature number) | UC-1.05 | W0 Table 1, `list-of-use-case.md` |
| Feature | `F-nn` (`F-00` = platform and access) | F-01 | W0 Table 2 |
| Function (L2) | `FR-<ff>-<nn>` | FR-01-02 | W1 |
| Work item (L3) | `FR-<ff>-<nn>-FE`, `-BE` or `-INT`, with a digit when one function has several of the same layer | FR-01-02-INT, FR-02-02-INT1 | W1 |
| Sequence diagram | `S-nn` (file) and `S-X.YY` (message) | S-01, S-1.03 | `list-of-message-sequence-diagram.md` |
| Activity diagram | `AD-UC-<f.nn>` | AD-UC-1.05 | `Docs/Activity Diagram/` |
| Business rule | `BR-<TOPIC>-nn` | BR-VFVR-01, BR-SOH-02 | the business rules table of the AD that applies it |
| Screen | `S-F<ff>-<nn>` (`S-F00-nn` = shared) | S-F01-02 | Screen List |
| Screen rule | `SR-nnn`, numbered inside one Screen Spec | SR-004 | that Screen Spec |

`S-nn`, `S-X.YY` and `S-Fxx-nn` look alike but are three different things (sequence diagram,
sequence message, screen). The glossary has a look-alike table.

The chain that every change must keep intact:

```text
P -> StR -> SyR -> UC -> F -> FR (EARS) -> AD (cites FR on each system action) -> S (cites UC and FR)

CTX flow -> UC -> SD message      (kept in Docs/Traceability/traceability.md)
```

## 5. How to do common tasks

**Before any task:** read this README, `Glossary.md`, `Project Brief.md`, then the owning files of
the facts the task touches (section 3).

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

**Add or change a business rule.** Edit the business rules table of the AD that owns it, give it
the next free number for its topic, and cite the ID (not the text) in the Screen Spec rules that
depend on it.

**Build a screen (DBIZ3).** Read in this order: the Screen Spec, the FRs it lists, the AD whose
actions it covers, the UI conventions file of that actor. Build every state in the Screen Spec.
The acceptance tests are the EARS criteria of the FRs and the branches in the AD.

**Add a term or abbreviation.** Add one row to `Glossary.md` in the right group: what it stands
for, what it means in this project, and where it is used. If two meanings collide, add a row to
the look-alike table too.

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
  `usecase-beta` and the activity diagrams use `swimlane-beta`, which need a recent Mermaid
  version; if a block does not render, paste it into [mermaid.live](https://mermaid.live).
- Car Owner interface text is in Vietnamese only, addressing the user as "bạn"
  (`Ui-conventions-car-owner.md`). Documents are written in English; proper names such as VIN,
  VETC, Carpla Service and SoH stay as they are.
- Dates `dd/MM/yyyy`, times `HH:mm` (24-hour), money in VND with dots for thousands and the suffix
  `₫` (`850.000.000 ₫`), mileage as `45.230 km`, percentages as whole numbers (`92%`).
- Person-hours, rates and thresholds follow the class Parameters of the Lesson 5 workbook. The
  benefit figures in W6 are team estimates for teaching.
- `[NEEDS CLARIFICATION: ...]` marks a question nobody has answered yet. It is never a decision.

## 7. State of this package

An agent must not treat the gaps below as decided, and must not fix them silently. The team should
update this list before each milestone.

**Scope and coverage**

- External systems the product reads (VETC, Carpla Service, the electronic invoice portal, EV
  charging stations, the inspection, traffic fine and secured transaction portals, partner
  marketplaces) are read-only data sources (W0 Table 4). Do not design writes to them.
- `Docs/Activity Diagram/` has 16 diagrams (`AD-UC-1.01`, `1.03`, `1.05`, `1.06`, `1.07`, `2.04`,
  `2.05`, `2.07` to `2.10`, `3.01`, `3.03` to `3.06`). The other use cases of the 30 in W0 Table 1
  are recorded in `Activity Index.md` with the reason they have no diagram.
- Screen Specs exist for `S-F01-01`, `S-F01-02`, `S-F01-03`, `S-F01-04`, `S-F01-06`, `S-F02-01`,
  `S-F03-01`, `S-F03-02`, `S-F03-03` and `S-F03-04`. `S-F00-01` (Home) has no Screen Spec yet.

**Known inconsistencies** (each should become a `[NEEDS CLARIFICATION: ...]` row where it blocks work)

- **Maintenance History screen ID.** `Screen List.md`, `Screen Transition.md` and the Car Owner usage
  flow call it `S-F01-05`; the Screen Spec file, its title and `AI Log.md` call it `S-F01-04`, and the
  modals table in `Screen List.md` also lists `S-F01-04`. Until the team picks one, do not add or
  renumber screens of F-01.
- **Screen Spec file names** do not follow one pattern. Current names: `screen spec S-F01-01.md`,
  `screen spec S-F01-02.md`, `screen spec S-F01-03.md`, `screen-spec-S-F01-04.md`,
  `screen-spec-S-F01-06.md`, `screen-spec-S-F02-01.md`, `screen-spec-S-F03-01.md`,
  `screen-spec-S-F03-02.md`, `screen-spec-S-F03-03.md`, `screen spec-S-F03-04.md`. The target
  pattern is `screen-spec-S-Fxx-nn.md`; rename them in one commit, then fix links.
- **Title of `screen spec-S-F03-04.md`** reads "S-F03-03 View preliminary quote"; the Screen List
  gives that screen the ID `S-F03-04`.
- **W1 work-item IDs in F-02** do not follow their parent function: for example `FR-02-05-BE` belongs
  to `FR-02-03`, and `FR-02-09-FE` belongs to `FR-02-10`. `FR-02-08` and `FR-02-09` have the same
  name ("Backfill 12-week price history"). Cite the L2 function ID and the exact L3 ID from W1; do
  not infer one from the other.
- **W2 and W5 are not finished.** The threshold check in `w2-cost.md` ends with a REVIEW verdict on the
  function-point cross-check, and its "three observations" are not filled in. The two open questions
  in `w5-handover.md` section 4 mention workstations and card-swipe data and appear to come from
  another project; do not use them.
- **Links.** `Project Brief.md`, `CVCA.md` and `Architecture.md` link to old commit URLs and old
  paths, and the Overview Use Case Diagram links to `../Functionlist/w0-scope.md` (the real folder is
  `Function List`). Use the paths in section 2.
- `CVCA.md` still carries the earlier title "Verified Vehicle Passport". The product name is
  Verified Vehicle Report (`F-01` is called Vehicle Passport).

**Open questions** marked `[NEEDS CLARIFICATION: ...]` are listed in the AD and Screen Spec files;
those with an answer show it in the last column. ADR-003 also needs confirmation from Tasco and
Carpla that the CRM can receive the report package and return the quote.

## 8. Reading order for people

| Step | Lesson | File | What it shows |
|---|---|---|---|
| 0 | | [Glossary.md](Glossảy.md) | Every abbreviation and project term, in one place |
| 1 | L1-L2 | [Project Brief.md](Docs/Project/Project%20Brief.md) | Client, problems, goals, team roles, milestones |
| 2 | L3 | [CVCA.md](Docs/Context/CVCA.md) | Stakeholders, value flows, problems, stakeholder requirements |
| 3 | L3 | [Context Diagram.md](Docs/Context/Context%20Diagram.md), [list-of-flow-context-diagram.md](Docs/Traceability/list-of-flow-context-diagram.md) | The system as a black box, the entities around it, every flow |
| 4 | L4 | [Overview Use Case Diagram](Docs/Use%20Case%20Diagram/Overview%20Verified%20Vehicle%20Report%20System%20Use%20Case%20Diagram.md), [UC-1](Docs/Use%20Case%20Diagram/UC-1.md), [UC-2](Docs/Use%20Case%20Diagram/UC-2.md), [UC-3](Docs/Use%20Case%20Diagram/UC-3.md), [list-of-use-case.md](Docs/Traceability/list-of-use-case.md) | Who wants the system to do what, per feature |
| 5 | L4 | [Sequence Diagram folder](Docs/Sequence%20Diagram/), [list-of-message-sequence-diagram.md](Docs/Traceability/list-of-message-sequence-diagram.md) | How the parts talk for each use case |
| 6 | L4 | [traceability.md](Docs/Traceability/traceability.md) | Context flow, use case and sequence messages in one table |
| 7 | L4 | [Architecture.md](Docs/Architecture/Architecture.md), [adr/](Docs/Architecture/adr/) | System requirements, design, six decisions |
| 8 | L5 | [w0-scope.md](Docs/Function%20List/w0-scope.md) | Use case inventory, features, Won't-have, data functions |
| 9 | L5 | [w1-function-list.md](Docs/Function%20List/w1-function-list.md) | Every function with its EARS criterion, split into work items |
| 10 | L5 | [w2-cost.md](Docs/Function%20List/w2-cost.md), [w4-priorities.md](Docs/Function%20List/w4-priorities.md), [w5-handover.md](Docs/Function%20List/w5-handover.md) | Cost, priorities, threshold checks, scenarios |
| 11 | L6 | [w6-cash-flow.md](Docs/Function%20List/w6-cash-flow.md) | Cash flow, NPV, IRR, payback |
| 12 | L7 | [Activity Index.md](Docs/Activity%20Diagram/Activity%20Index.md), [Activity Diagram folder](Docs/Activity%20Diagram/) | Inputs check, then activity diagrams with business rules |
| 13 | L8 | [Car owner usage flow](Docs/External%20Design/Usage-flow-tasco-verifed-vehicle-report-system.md), [Carpla CRM usage flow](Docs/External%20Design/Usage-flow-carpla-crm.md) | One usage flow per actor |
| 14 | L8 | [Screen List.md](Docs/External%20Design/Screen%20List.md), [Screen Transition.md](Docs/External%20Design/Screen%20Transition.md) | Screens and how users move between them |
| 15 | L8 | [Ui-conventions-car-owner.md](Docs/External%20Design/Ui-conventions-car-owner.md), [Ui-conventions-carpla-staff.md](Docs/External%20Design/Ui-conventions-carpla-staff.md) | Language, formats, components, design tokens |
| 16 | L8 | [Screen folder](Docs/External%20Design/Screen/) | Screen Specs with state images |
| all | all | [AI Log.md](Docs/Project/AI%20Log.md) | Where AI suggested something wrong, and what the team did |
