# BIM QA/QC Wall Type Checker | Dynamo + Python + Revit API

A Dynamo + Python automation tool for Autodesk Revit that performs wall type classification, parameter writing, and Excel-based QA/QC reporting.

## Overview

This tool automates a repetitive BIM coordination task by checking wall type names against a user-defined keyword and automatically classifying wall elements inside Revit.

The workflow also generates an Excel report containing wall IDs, wall type names, and QA/QC results.

## Features

* Collects all wall elements from the Revit model
* Reads wall type names using the Revit API
* Applies keyword-based classification logic
* Writes results into the Comments parameter
* Generates Excel QA/QC reports
* Uses reusable Dynamo Python inputs

## Inputs

| Input | Description                      | Example          |
| ----- | -------------------------------- | ---------------- |
| IN[0] | Keyword to search                | Exterior         |
| IN[1] | Status if keyword matches        | EXTERIOR WALL    |
| IN[2] | Status if keyword does not match | INTERIOR / OTHER |

## Workflow

1. Open Revit model
2. Run Dynamo graph
3. Enter keyword and status inputs
4. Execute Python script
5. Review updated Comments parameters
6. Export QA/QC report to Excel

## Excel Output

| Wall ID | Wall Type           | Status           |
| ------- | ------------------- | ---------------- |
| 12345   | Basic Wall Exterior | EXTERIOR WALL    |
| 67890   | Generic Interior    | INTERIOR / OTHER |

## Technologies Used

* Autodesk Revit
* Dynamo
* Python
* Revit API
* Excel Integration

## Skills Demonstrated

* BIM QA/QC automation
* Revit API integration
* Parameter reading and writing
* Custom filtering logic
* Excel report generation
* Reusable automation workflows

## Future Improvements

* WPF user interface
* Multi-category support
* CSV/JSON export
* Automatic issue dashboards
* ACC integration
