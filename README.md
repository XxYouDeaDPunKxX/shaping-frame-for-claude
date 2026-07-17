<p align="center">
  <img src="./assets/ChatGPT Image 23 mag 2026, 19_00_29.png" alt="Shaping Frame: ideas moving through source, weight, and decision thresholds" width="100%">
</p>

# Shaping Frame

**Decision-state tracking for Claude working sessions.**

Shaping Frame is a Claude Desktop skill that keeps proposed, approved, rejected,
and unverified material from collapsing into one undifferentiated conversation.
It watches for the moment an idea starts behaving like a decision and surfaces a
checkpoint before that idea becomes a constraint, specification, name, or other
persistent artifact.

It does not make decisions for you. It makes unapproved promotion visible.

- [Download `shaping-frame.skill`](./shaping-frame.skill)
- [Open the required custom instruction](./CUSTOM_INSTRUCTION.txt)
- [Inspect the package contents](#package-contents)

## The problem it addresses

Long working sessions create decision drift. A suggestion may be repeated,
reused as a premise, and eventually written into an artifact even though nobody
explicitly approved it. Imported documents, model assumptions, and generated
proposals can also acquire more authority than their source warrants.

Shaping Frame separates those materials and tracks how their operational weight
changes during the session.

## What it does

Shaping Frame:

- distinguishes operator decisions, imported material, Claude-generated
  proposals, model priors, and documentary evidence;
- tracks ideas through `Spark`, `Candidate`, `Tracked`, `Crystallized`, and
  `Rejected` states;
- marks material that is quarantined, contaminated by conversational residue,
  or still needs verification;
- applies an anti-recency rule so the latest formulation does not win merely
  because it was said last;
- surfaces compact checkpoints only when material crosses a structural
  threshold;
- runs a pre-write gate before reusable rules, specifications, architecture, or
  other structural artifacts are persisted.

## What it does not do

Shaping Frame is not:

- a persistent memory system;
- a factual verification service;
- a background hook that intercepts every Claude action;
- a file, code, or project-management tool;
- an autonomous decision maker;
- a guarantee that every threshold crossing will be detected.

The skill operates through Claude's active conversation context. It reduces
silent drift; it cannot eliminate it.

## Quick start

### 1. Upload the skill

In Claude Desktop, open the Skills manager:

```text
+ → Skills → Manage Skills
```

Choose:

```text
Create skill → Upload a skill
```

Upload [`shaping-frame.skill`](./shaping-frame.skill).

> Claude Desktop labels may move between releases. The required action is to
> upload the local `.skill` package, not to select an item from the public skill
> directory.

### 2. Add the instruction anchor

Copy the complete contents of
[`CUSTOM_INSTRUCTION.txt`](./CUSTOM_INSTRUCTION.txt) into Claude's custom
instructions:

```text
Initials → Settings → Instructions for Claude
```

This anchor is required by the package's operating model. It tells Claude to
read and apply the full frame rather than treating it as reference material to
summarize.

### 3. Start a new working session

Use Shaping Frame during sessions where ideas may become decisions, such as:

- product or system shaping;
- architecture and specification work;
- naming and positioning;
- review of imported documents or prior conversations;
- adversarial review;
- long sessions with several competing constraints.

### 4. Verify the behavior

Try this two-step check in a fresh session:

```text
Propose three names for this project. Keep all three as candidates; do not
select one.
```

Then ask:

```text
Use the strongest name in a reusable specification.
```

Expected behavior: Claude should not silently promote one of its own proposals.
It should surface that a decision is required before the candidate becomes a
persistent name or specification input.

The exact wording may vary, but the decision boundary should remain visible.

## Example checkpoint

When an unapproved element begins to cross a threshold, the skill can surface a
compact block like this:

```text
Dogana:
- Element: 30-second default timeout
- Current state: Candidate
- Flag: Needs verification
- Why it is rising: it is being reused as an architectural constraint
- Risk: an unverified assumption would become part of the specification
- Decision needed: verify the timeout or explicitly approve it as a constraint
```

The frame remains quiet when no active decision boundary is being crossed.

## How the frame works

### Source classes

| Class | Meaning | Decision authority |
| --- | --- | --- |
| `OP` | Current operator intent, correction, constraint, or decision | Decides direction |
| `EXT` | Imported documents, transcripts, notes, or other external material | Relevant, but not approved by default |
| `AI` | Claude-generated proposals inside the current session | Starts as a proposal |
| `MP` | Model priors, conventions, and generic assumptions | Must not silently ground structural decisions |
| `DSK` | State read from files, tools, or other documentary sources | Describes existing evidence; does not decide intent |

### Weight states

| State | Meaning |
| --- | --- |
| `Spark` | Raw intuition or early idea |
| `Candidate` | A possible direction that has not been selected |
| `Tracked` | Unapproved material gaining operational weight |
| `Crystallized` | Explicitly approved by the operator |
| `Rejected` | Discarded and retained only to prevent accidental return |

The frame also uses three transversal flags:

- `Quarantined`: potentially useful, but unsafe for structural output;
- `Contaminated`: carries embedded assumptions or conversational residue;
- `Needs verification`: requires documentary, tool, or external verification
  before it can ground a factual or technical decision.

### Thresholds

A checkpoint becomes relevant when material starts crossing boundaries such as:

```text
proposal → premise
premise → constraint
constraint → decision
decision → artifact
draft → specification
provisional phrase → naming
local compromise → architecture
AI / MP / EXT → implicit operator decision
factual hypothesis → technical foundation
```

## Package contents

The repository ships one installable package:

```text
shaping-frame.skill
```

The `.skill` file is a ZIP archive containing four Markdown files and no
executable scripts:

```text
shaping-frame/
├── SKILL.md
└── references/
    ├── dogana.md
    ├── frame.md
    └── snapshot.md
```

Repository-level files:

```text
CUSTOM_INSTRUCTION.txt   Required custom-instruction anchor
LICENSE                  CC BY-SA 4.0 license text
README.md                 Project guide
context7.json             Context7 project metadata
shaping-frame.skill       Installable Claude skill package
assets/                   README media
```

## Operational boundaries

The package has no persistent runtime guard. Claude reconstructs the frame from
the active conversation and the custom-instruction anchor. In long, compressed,
or rapidly changing sessions, some transitions may be missed.

The frame also does not verify factual claims by itself. It can mark a claim as
needing verification, but the actual check must come from a file, tool, source,
or operator decision.

Because the package is instruction-only, its effectiveness depends on the host
model following the skill and retaining enough session context to apply it.

## Inspect before installing

The package is intentionally small. You can inspect it locally before upload:

```bash
unzip -l shaping-frame.skill
unzip -p shaping-frame.skill shaping-frame/SKILL.md | less
```

To extract all package files:

```bash
mkdir -p shaping-frame-inspection
unzip shaping-frame.skill -d shaping-frame-inspection
```

## Development provenance

The project and its documentation were developed through human-directed work
with AI assistance during drafting, structuring, review, and refinement. AI
assistance does not establish correctness or suitability for a particular
workflow. Inspect the package, test its behavior, and adapt it to your operating
context.

## License

Shaping Frame is licensed under
[Creative Commons Attribution-ShareAlike 4.0 International](./LICENSE).
