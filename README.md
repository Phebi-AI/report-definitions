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

The elements is where all the magic happens. An element can be chosen from a range of pre-defined elements, or even custom developed ones (for more details about custom developed elements, check out [this](https://github.com/Phebi-AI/charting-sample-1) example).

Which settings are available can differ to the element type. You can find a full list in the [Phebi Report-Elements Reference](#)

```
{
  "Id": "filter_Segment",
  "Type": "Filter_Select",
  "FilterType": "Single",
  "Label": "Gender",
  "Values": "[Gender]",
  "AllOption": true,
  "Custom": false,
  "Controls": [ "emotional_resonance" ]
}
```

### Values syntax

The values syntax is used across different report elements and defines filters and what data to pass to the report element.

```
{
  "Filter": "eval('[Gender]' == 'Male')",
  "GroupBy": "[Market]"
}
```

Square brackets retrieve a value from a data entry that can be used to compare to another value or a fixed data type. Fields are basic fields like respondent, sentiment and emotion scores, or tags that have been assinged to the data entry either manually through the user, or in a bulk import.

A full list of available default fields can be found in the [Phebi Report-Elements Reference](#)

#### Other functions

There are a number of functions available to the user. Mainly very useful when combined with the "GroupBy" functionality. That way the system can calculate averages or frequencies that then can be passed to basic reports like a column or pie chart.

```
{
  "Id": "chart1",
  "Type": "Column"
  "Filter": "eval('[Gender]' == 'Male')",
  "GroupBy": "[Market]",
  "Dimension": "[Market]",
  "Measure": "Sentiment",
  "Measures": [
    { "Name": "Market", "Value": "[Market]" },
    { "Name": "Sentiment", "Value": "average([Sentiment.Compound])" }
  ]
}
```

The available functions are:
Function|Description|Values Example|Returns Example|Returns type
------------ | ------------- | ------------- | ------------- | -------------
frequency(|Returns the value with the highest frequency of a given field.|"Value": "frequency([Market])"|Italy|string
count(|Counts the occurrences of a field.|"Value": "count([City])"|Rome|string
total(|Returns the sum of the given input syntax|"Value": "total([ERS.Positive])"|15.2|float
eval(|Evaluates the given input query.|"Value": "total(eval('[Market]' == 'Germany' ? 1 : 0))"|10|float
average(|Returns the average of the given input syntax|"Value": "average([ERS.Positive])|0.21514|float
round(|Rounds a given number|"Value": "average(eval(round([ERS.Positive] * 10) / 10))"|0.21|float

#### Custom functions

Anything that is not possible using the values syntax can be achieved using custom javascript code.

```
"GetData": "(function(data) { var customScores = [1.25,0.98,0.75,2.65,1.35]; var result = []; for (var i = 0; i < data.length; i++) { var emotions = []; for (var e = 0; e < data[i].Emotion.length; e++) { emotions.push(data[i].Emotion[e]); } result.push({ "Respondent": data[i].Respondent, "CustomEmotionScore": emotions }); } })"
```
