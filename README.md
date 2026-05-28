# BIM QA/QC Wall Type Checker

A Dynamo + Python automation tool for Autodesk Revit that checks wall types based on user-defined keywords and writes QA/QC results into the Comments parameter.

## Problem

In BIM coordination workflows, checking whether wall types follow naming or classification logic is often done manually. This process is repetitive, time-consuming, and prone to human error.

## Solution

This tool automates the wall type checking process by:

- Collecting all wall elements in the Revit model
- Reading each wall type name
- Checking whether the type name contains a user-defined keyword
- Writing the result into the Comments parameter
- Producing a summary report in Dynamo

## Inputs

| Input | Description | Example |
|---|---|---|
| IN[0] | Keyword to search in wall type name | Exterior |
| IN[1] | Status if keyword is found | EXTERIOR WALL |
| IN[2] | Status if keyword is not found | INTERIOR / OTHER |

## Workflow

1. Open Revit model
2. Run Dynamo graph
3. Enter keyword and status values
4. Run script
5. Check wall Comments parameter
6. Review output report

## Output

Example output:

```text
Checked 120 walls | Matched: 48 | Not matched: 72
