# [Scenario Name] — [Difficulty/Type]

**Date:** YYYY-MM-DD
**Status:** [Investigating / Resolved / Blocked]

## Symptom
*   **Error Message:** `[Exact error text, copy-pasted]`
*   **Behavior:** [What was expected vs. what actually happened]

## Investigation Log
*   **Hypothesis 1:** [What you suspected and why]
    *   **Test:** [Command or action you took]
    *   **Result:** [What you found]
*   **Hypothesis 2:** [Next suspicion]
    *   **Test:** [Command or action]
    *   **Result:** [What you found]
*   **Failed Attempts:** [Note what you tried that didn't work and why, to avoid dead ends in the future]

## Root Cause
[The actual technical reason for the failure, not just the symptom]

## Solution
*   **Fix:** [The minimal change that resolved it]
*   **Verification:** [How you confirmed it was fixed, e.g., `systemctl status`, `check.sh` output]

## Lesson / Pattern
[What is the reusable takeaway? e.g., "A full disk can exist even if `du` says otherwise; always check `lsof +L1`."]