# ACP Plan Standard Template v1.0.0 (Compact)

Use this template for bounded, low-risk, semantically narrow work.
Use it only when concise expression still keeps execution strategy, affected scope,
and later Change compatibility clear.

Compact means concise expression, not reduced ACP meaning.

# PLAN: <plan_id>

## Meta
- Related Request: <request_id>
- Summary: <one-line execution summary; not a result report>
- Primary Capability: <capability_id or none>
- Target Slice: <slice_id | _root | none>
- Affected Modules:
  - <module_or_path directly affected by this plan>
- User Confirmation: <true | false>
- Created At: <ISO8601 timestamp>
- Status: <draft | confirmed | executing | done | cancelled | superseded>

## Goal and Boundary
- Original Need:
- Agent Understanding: <structured understanding of goal and constraints>
- In Scope: <only the work this compact plan will perform>
- Out of Scope: <explicitly excluded work or follow-up>

## Execution Strategy
- Strategy Summary: <how this plan will satisfy the request; concise but execution-relevant>
- Route / Entry Points: <starting capability, module, or entry file; not a full inventory>

Add only the step blocks required for execution.

### Step 1
- Objective:
- Target Files: <likely touch points only>
- Patch Intent: <what will change and why>
- Verification: <observable check for completion>

### Step 2 (Optional)
- Objective:
- Target Files: <likely touch points only>
- Patch Intent: <what will change and why>
- Verification: <observable check for completion>

## Validation and Risk
- Acceptance Criteria: <conditions required for success and safe closure>
- Main Risks: <main regression, uncertainty, or scope risk only>
- Fallback Direction: <next safe path if this approach fails>
