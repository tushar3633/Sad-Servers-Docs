# "Saskatoon": counting IPs — Easy/Do

**Date:** 2026-11-18
**Status:** Resolved

## Symptom
*   **Behavior:** A web server access log (`/home/admin/access.log`) is present. I need to find the IP address with the highest number of requests and write *only* that IP address to `/home/admin/highestip.txt`.

## Investigation Log
*   **Hypothesis 1:** The IP address is the first column. I can use a standard pipeline to count occurrences of items in column 1.
    *   **Test:** `awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | head -n 1`
    *   **Result:** Successfully identified the winning IP and its count (e.g., `150 192.168.1.105`).
*   **Hypothesis 2:** The test script requires *only* the IP address in the file, with no leading spaces or counts. I need to strip the count.
    *   **Test:** Piped the output into a second `awk` to grab the 2nd column: `... | awk '{print $2}' > /home/admin/highestip.txt`
    *   **Result:** The file now contains only the IP address.

## Root Cause
N/A (Data analysis task).

## Solution
*   **Fix:** Used the Top-N pipeline with a secondary extraction step:
    `
    awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -nr | head -n 1 | awk '{print $2}' > /home/admin/highestip.txt`