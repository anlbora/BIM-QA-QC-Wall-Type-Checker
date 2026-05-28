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

'''
import clr

clr.AddReference("RevitServices")
from RevitServices.Persistence import DocumentManager
from RevitServices.Transactions import TransactionManager

clr.AddReference("RevitAPI")
from Autodesk.Revit.DB import FilteredElementCollector, Wall

doc = DocumentManager.Instance.CurrentDBDocument

search_text = IN[0]
match_status = IN[1]
non_match_status = IN[2]

walls = FilteredElementCollector(doc).OfClass(Wall).ToElements()

results = []
report_data = []

matched_count = 0
non_matched_count = 0

# Excel header
report_data.append(
    ["Wall ID", "Wall Type", "Status"]
)

TransactionManager.Instance.EnsureInTransaction(doc)

for wall in walls:

    wall_type = doc.GetElement(wall.GetTypeId())

    if not wall_type:
        continue

    type_name_param = (
        wall_type.LookupParameter("Type Name")
        or wall_type.LookupParameter("Tip Adı")
    )

    if type_name_param:
        type_name = type_name_param.AsString()
    else:
        type_name = "UNKNOWN TYPE"

    if search_text.lower() in type_name.lower():
        status = match_status
        matched_count += 1
    else:
        status = non_match_status
        non_matched_count += 1

    comments = wall.LookupParameter("Comments")

    if comments:
        comments.Set(status)

    results.append(
        "Wall ID {} → {} → {}".format(
            wall.Id.IntegerValue,
            type_name,
            status
        )
    )

    # Excel report row
    report_data.append(
        [
            wall.Id.IntegerValue,
            type_name,
            status
        ]
    )

    TransactionManager.Instance.TransactionTaskDone()

    summary = "Checked {} walls | Matched: {} | Not matched: {}".format(
        len(walls),
        matched_count,
        non_matched_count
    )

    OUT = report_data

    '''
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
