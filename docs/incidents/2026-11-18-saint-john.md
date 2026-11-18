# "Saint John": what is writing to this log file — Easy

**Date:** 2026-11-18 \
**Status:** [Resolved]

## Symptom
*   **Error Message:** N/A Program filling up disk space
*   **Behavior:** Process is continiously writing to /var/log/bad.log.

## Investigation Log
*   **Hypothesis 1:** Script running in background and writing to /var/log/bad.log.
    *   **Test:** 'sudo lsof /var/log/bad.log'
    *   **Result:** SHowed the process writing to the file
*   **Failed Attempts:** N/A

## Root Cause
A Python script running in background henl an open file descriptor to the log file , continiously appending data

## Solution
*   **Fix:** Terminated the process `kill -9 <PID>`
*   **Verification:** Ran `lsof /var/log/bad.log` again (no output) and confirmed with `tail -f` that the log file was no longer growing.

## Lesson / Pattern
**Deleting a file does not stop running process.** \
To stop the writing , find the process's PID with `lsof` and `kill -9 <PID>`