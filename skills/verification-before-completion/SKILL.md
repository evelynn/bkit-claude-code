---
name: verification-before-completion
description: Use when about to claim work is complete, fixed, or passing, before committing or creating PRs - requires running verification commands and confirming output before making any success claims; evidence before assertions always
---

# Verification Before Completion

## Overview

Claiming work is complete without verification is dishonesty, not efficiency.

**Core principle:** Evidence before claims, always.

**Violating the letter of this rule is violating the spirit of this rule.**

## The Iron Law

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

If you haven't run the verification command in this message, you cannot claim it passes.

## The Gate Function

```
BEFORE claiming any status or expressing satisfaction:

1. IDENTIFY: What command proves this claim?
2. RUN: Execute the FULL command (fresh, complete)
3. READ: Full output, check exit code, count failures
4. VERIFY: Does output confirm the claim?
   - If NO: State actual status with evidence
   - If YES: State claim WITH evidence
5. ONLY THEN: Make the claim

Skip any step = lying, not verifying
```

## bkit PDCA Integration

This skill integrates with bkit's PDCA workflow. Verification requirements differ by phase:

### PDCA Phase-Specific Verification

| PDCA Phase | Verification Required | bkit Tool |
|------------|----------------------|-----------|
| **Plan** | Requirements completeness check | `/pdca plan` output review |
| **Design** | Design document consistency | `design-validator` agent |
| **Do** | Build + Tests + Lint pass | Shell commands (see Key Patterns) |
| **Check** | Gap analysis Match Rate | `gap-detector` agent |
| **Act** | Re-verification after fixes | `pdca-iterator` agent |
| **Report** | All above verified before report | `report-generator` agent |

### bkit Verification Flow

```
BEFORE claiming PDCA phase completion:

1. Do phase complete?
   → Run build/test/lint commands → Verify ALL pass
   → Run gap-detector → Verify Match Rate

2. Match Rate < 90%?
   → DO NOT claim completion
   → Run pdca-iterator for auto-improvement
   → Re-verify after iteration

3. Match Rate >= 90%?
   → Verify the number is from FRESH gap-detector run
   → THEN claim completion
   → Generate report with report-generator

4. TaskUpdate status to "completed"?
   → ONLY after ALL verifications above pass
```

### Agent Trust Policy

```
bkit agent reports "success" or "Match Rate: 95%"
  → DO NOT trust blindly
  → Read the ACTUAL output
  → Verify the evidence yourself
  → Check for edge cases the agent might have missed
```

## Common Failures

| Claim | Requires | Not Sufficient |
|-------|----------|----------------|
| Tests pass | Test command output: 0 failures | Previous run, "should pass" |
| Linter clean | Linter output: 0 errors | Partial check, extrapolation |
| Build succeeds | Build command: exit 0 | Linter passing, logs look good |
| Bug fixed | Test original symptom: passes | Code changed, assumed fixed |
| Regression test works | Red-green cycle verified | Test passes once |
| Agent completed | VCS diff shows changes | Agent reports "success" |
| Requirements met | Line-by-line checklist | Tests passing |
| PDCA phase done | Fresh gap-detector run: >= 90% | Previous analysis result |
| Feature complete | All PDCA phases verified | "Do" phase tests passing alone |

## Red Flags - STOP

- Using "should", "probably", "seems to"
- Expressing satisfaction before verification ("Great!", "Perfect!", "Done!", etc.)
- About to commit/push/PR without verification
- Trusting agent success reports (including bkit agents)
- Relying on partial verification
- Thinking "just this once"
- Tired and wanting work over
- Claiming PDCA phase complete without running the phase's verification tool
- Using old gap-detector results as current evidence
- **ANY wording implying success without having run verification**

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Should work now" | RUN the verification |
| "I'm confident" | Confidence ≠ evidence |
| "Just this once" | No exceptions |
| "Linter passed" | Linter ≠ compiler |
| "Agent said success" | Verify independently |
| "I'm tired" | Exhaustion ≠ excuse |
| "Partial check is enough" | Partial proves nothing |
| "Different words so rule doesn't apply" | Spirit over letter |
| "Gap detector ran earlier" | Re-run NOW, stale results ≠ evidence |
| "Match Rate was 95% last time" | Was that BEFORE or AFTER your latest changes? |
| "PDCA report says complete" | Did YOU verify each section? |

## Key Patterns

**Tests:**
```
OK  [Run test command] [See: 34/34 pass] "All tests pass"
NG  "Should pass now" / "Looks correct"
```

**Regression tests (TDD Red-Green):**
```
OK  Write -> Run (pass) -> Revert fix -> Run (MUST FAIL) -> Restore -> Run (pass)
NG  "I've written a regression test" (without red-green verification)
```

**Build:**
```
OK  [Run build] [See: exit 0] "Build passes"
NG  "Linter passed" (linter doesn't check compilation)
```

**Requirements:**
```
OK  Re-read plan -> Create checklist -> Verify each -> Report gaps or completion
NG  "Tests pass, phase complete"
```

**Agent delegation:**
```
OK  Agent reports success -> Check VCS diff -> Verify changes -> Report actual state
NG  Trust agent report
```

**bkit Gap Analysis:**
```
OK  [Run gap-detector] [See: Match Rate 92%] "Gap analysis passed"
NG  "I think we covered all the design requirements"
```

**bkit PDCA Completion:**
```
OK  [Build OK] + [Tests OK] + [gap-detector >= 90%] -> "Phase complete"
NG  "Implementation looks done, generating report"
```

## When To Apply

**ALWAYS before:**
- ANY variation of success/completion claims
- ANY expression of satisfaction
- ANY positive statement about work state
- Committing, PR creation, task completion
- Moving to next task or next PDCA phase
- Delegating to agents
- Running report-generator
- Marking TaskUpdate as "completed"

**Rule applies to:**
- Exact phrases
- Paraphrases and synonyms
- Implications of success
- ANY communication suggesting completion/correctness

## The Bottom Line

**No shortcuts for verification.**

Run the command. Read the output. THEN claim the result.

This is non-negotiable.
