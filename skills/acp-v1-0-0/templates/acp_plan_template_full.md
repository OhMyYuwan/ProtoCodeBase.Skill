# ACP Plan Standard Template v1.0.0 (Full)

Use this template for complex, multi-step, cross-module, or diagnosis-heavy work.
Use it when light would hide material uncertainty, dependency structure,
cross-surface impact, or revision-sensitive planning state.

Full means a complete execution blueprint with deeper execution and analysis detail.
It is not a review form, handoff note, or pre-execution report.

# PLAN: <plan_id>

## Meta
- Related Intent: <intent_id or none; include only if tracked>
- Related Request: <request_id>
- Summary: <one-line execution summary; not a result report>
- Primary Capability: <capability_id or none>
- Related Capabilities:
  - <capability_id materially involved beyond the primary target>
- Target Slice: <slice_id | _root | none>
- Affected Modules:
  - <module_or_path directly or materially affected>
- User Confirmation: <true | false>
- Created At: <ISO8601 timestamp>
- Status: <draft | confirmed | executing | done | cancelled | superseded>
- Risk Level: <low | medium | high | none; planning risk, not outcome report>
- Plan Revision: <integer; use current revision number>
- Supersedes Plan: <plan_id or none>
- Plan Change Type: <initial | editorial | material>
- Request Alignment Status: <aligned | pending_sync | superseded | not_applicable>

## Goal and Boundary
- Original Need:
- Agent Understanding: <structured understanding of goal and constraints>
- Expected Direction: <high-level satisfaction path, not step detail>
- Expected Outcome: <intended end state after execution>
- User-Visible Impact: <visible behavior or workflow change; omit if internal-only>
- In Scope: <work this plan is expected to carry out>
- Out of Scope: <excluded work, deferred depth, or non-goals>

## Strategy and Analysis
- Keep this section execution-relevant. Do not expand it into a review form.
- Strategy Summary: <why this execution route is preferred at full detail>
- Route / Entry Points: <starting capability, module, or entry file set>
- Observed Symptoms:
- Working Analysis: <current diagnosis or system interpretation>
- Working Hypotheses: <plausible causes or working assumptions; omit if not needed>
- Preferred Path:
- Alternative Paths:

Add only the step blocks required for execution. Duplicate as needed.

### Step 1
- Objective:
- Capability: <primary semantic target for this step>
- Slice: <slice or _root if relevant>
- Depends On: <prior step, artifact, or none>
- Priority: <critical | high | normal | low>
- Target Files / Modules: <likely touch points, not exhaustive repo listing>
- Execution Notes: <how this step should be carried out; detailed but execution-relevant>
- Patch Intent: <what will change and why>
- Optional Pseudocode / Replacement Sketch: <only when prose is insufficient; avoid full implementation dumps>
- Expected Intermediate Result: <state expected after this step, if relevant>
- Verification: <observable check for completion>

### Step 2 (Optional)
- Objective:
- Capability: <primary semantic target for this step>
- Slice: <slice or _root if relevant>
- Depends On: <prior step, artifact, or none>
- Priority: <critical | high | normal | low>
- Target Files / Modules: <likely touch points, not exhaustive repo listing>
- Execution Notes: <how this step should be carried out; detailed but execution-relevant>
- Patch Intent: <what will change and why>
- Optional Pseudocode / Replacement Sketch: <only when prose is insufficient; avoid full implementation dumps>
- Expected Intermediate Result: <state expected after this step, if relevant>
- Verification: <observable check for completion>

## File Matrix
| File / Module | Planned Action | Related Step | Capability / Slice | Risk |
|---|---|---|---|---|
| <path> | modify / create / remove / rename | Step 1 | <capability/slice> | low |
| <path> | modify / create / remove / rename | Step 2 | <capability/slice> | medium |

## Validation and Risk
- Validation Plan: <how completion will be checked at execution close>
- Acceptance Criteria: <conditions required for success and safe closure>
- User-Facing Success Signal: <how the user can tell it worked; omit if internal-only>
- Failure Signals: <signs that the plan did not succeed or should not close>
- Main Risks: <main execution, regression, dependency, or scope risks>
- Trade-Offs: <meaningful trade-offs only>
- Fallback Direction: <next safe path if this approach fails>
