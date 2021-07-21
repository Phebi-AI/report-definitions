# Phebi Report Definition

## 1. How Reports work

In Phebi, a report is a set of defined report elements.. In order to create a custom report in Phebi, we have to create a report element that then is referred to in a report definition file.

## 2. The Report Definition File

The report definition is quite simple. It mainly consists of rows, columns and elements. It will also need a title and a thumbnail image. There are a number of default report elements that can be configured through a set of settings.

```
{
  "Title": "ERS",
  "Preview": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAAAMSURBVBhXY/j//z8ABf4C/qc1gYQAAAAASUVORK5CYII=",
  "Rows": [
    {
      "height": "100px",
      "Columns": [
        {
          "width": "auto",
          "Elements": [
            {
              "Id": "filter_Segment",
              "Type": "Filter_Select",
              "FilterType": "Single",
              "Label": "Type",
              "Values": "[Type]",
              "AllOption": true,
              "Custom": false,
              "Controls": [ "emotional_resonance" ]
            }
          ]
        },
        {
          "width": "auto",
          "Elements": [
            {
              "Id": "filter_Country",
              "Type": "Filter_Select",
              "FilterType": "Single",
              "Label": "Market",
              "Values": "[Market]",
              "AllOption": true,
              "Custom": false,
              "Controls": [ "emotional_resonance" ]
            }
          ]
        },
        {
          "width": "auto",
          "Elements": [
            {
              "Id": "filter_respondent",
              "Type": "Filter_Select",
              "FilterType": "Single",
              "Label": "Respondent",
              "Values": "[Respondent]",
              "AllOption": true,
              "Custom": false,
              "Controls": [ "emotional_resonance" ]
            }
          ]
        }
      ]
    },
    {
      "height": "auto",
      "Columns": [
        {
          "width": "auto",
          "Elements": [
            {
              "Id": "emotional_resonance",
              "Type": "ERS",
              "Dimension": "Section",
              "Class": "DashboardElement_NoBox"
            }
          ]
        }
      ]
    }
  ]
}
```

### Title & Preview Image

The Report requires a name and thumbnail image encoded in base64 string. This is how the report will appear in the Phebi Portal under reports. 

```
"Title": "ERS",
"Preview": "data:image/png;base64,ImageAsBase64String"
```

### Rows

The report is rendered like a table. It starts with rows and then columns within the rows.

```
"Rows": [
  {
    "height": "auto",
    "Columns": []
  }
]
```

By default the row height is set to auto. That means it will distribute the available height to all rows with height set to "auto". If there is a row with a fixed height, for example for filters, the report renderer will substract the fixed height from the available space and distribute the remaining space to the rows with "auto" height.

You can also define a min-height, to prevent the automatically distrubuted height dropping below a certain point on small screens:

```
"Rows": [
  {
    "height": "auto",
    "min-height": "100px",
    "Columns": []
  }
]
```

### Columns

Very similarly to the rows, the columns are defined. The "width" attribute works the same way as the height in the rows. When set to "auto", it will distribute the available screen width to all columns equally.

```
"Rows": [
  {
    "height": "auto",
    "min-height": "100px",
    "Columns": [
      {
        "width": "auto",
        "Elements": []
      }
    ]
  }
]
```

### Elements

Now the elements is where all the magic happens. An element can be chosen from a range of pre-defined elements, or even custom developed ones (for more details about custom developed elements, check out [this](https://github.com/Phebi-AI/charting-sample-1) example).


