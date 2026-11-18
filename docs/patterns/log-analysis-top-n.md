# Pattern: Top-N Log Analysis Pipeline

**Scenario Reference:** [Saskatoon](../incidents/2026-11-18-saskatoon.md)

## The Problem
You need to analyze a text file or log file to find the most frequent occurrence of a specific item (like an IP address, an HTTP status code, or a username).

## The Solution Pipeline
The standard Linux pipeline to find the "Top N" items in a log file is:

`command | sort | uniq -c | sort -nr | head -n N`

### Step-by-Step Breakdown:
1. **Extract:** Get the specific column you want.
   * `awk '{print $1}' logfile` (gets the 1st column)
   * `cut -d',' -f3 logfile` (gets the 3rd column in a CSV)
2. **Group:** `sort` 
   * Groups identical adjacent lines together (required for `uniq` to work).
3. **Count:** `uniq -c`
   * Removes duplicates and prepends the count as the **first column**.
4. **Rank:** `sort -nr`
   * Sorts numerically (`-n`) in reverse/descending order (`-r`). Highest count floats to the top.
5. **Limit:** `head -n 5`
   * Shows only the top 5 results. Change to `head -n 1` if you only want the #1 winner.

## Extracting Only the Winning Result
If a script requires *only* the winning item (not the count), extract the second column from the final output:
`... | head -n 1 | awk '{print $2}'`

## Common Use Cases
*   **Most frequent IP:** `awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -n 1`
*   **Most frequent HTTP Status (like 404s):** `awk '{print $9}' access.log | sort | uniq -c | sort -nr | head -n 1`