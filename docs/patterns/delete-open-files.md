# Pattern: Deleted but Open Files

**Scenario Reference:** [Saint John](../incidents/2026-11-18-saint-john.md)

## The Problem
You delete a large log file or script, but `df -h` still shows the disk is full, or the process keeps running. 

## Why it happens
Linux allows a process to keep a file open even after it has been deleted from the filesystem. The disk space is not freed until the process is terminated.

## How to detect it
Use `lsof +L1` to list all open files with a link count of 0 (meaning they have been deleted but are still held open). Or use `lsof /path/to/file`.

## How to fix it
Find the PID holding the file open and kill the process (`kill <PID>`). 