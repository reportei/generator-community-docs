# Widget Structure in the Generator

After determining that a metric is available via the integration API, we need to create the "skeleton" of the widget that will contain each metric in a report.

The items in the widgets are exactly the payload which is sent to the generator `/source` endpoint

## Base Widget Types

There are only three base widget types, as they can be used to create other widgets:

1. **Datatable**
2. **Number**
3. **Chart**

Each widget property serves a specific purpose.

### Dimensions and Metrics (Most Important!)
These must precisely represent the resources or fields that the metric will use from the API.

For example:
- If a widget has `user` as a dimension, it must make a request to the API's `user` endpoint.
- If its metrics include `save`, the widget will retrieve `save` metrics from the `user` resource.

A widget can have multiple dimensions and metrics, usually indicating that:
- It consumes multiple API resources.
- It is segmented in multiple ways.
- It utilizes those resources for calculations.

**Key Rule:** `metrics` and `dimensions` must align with the API resources being consumed. Do not add a dimension arbitrarily. More details on this process can be found in the [Adding a new integration](./new_integration.md) guide in Reportei.

#### Important Note for Tables
For tables, columns must always follow the structure:

```plaintext
[dimension, metric1, metric2, metric3]
```

The total number of columns must equal the sum of dimensions and metrics.

### Widget Properties

#### `reference_key`
Each widget must have a unique key following this format:

```plaintext
network-name:metric
```

Example:
```plaintext
pinterest:pins
```

#### `component`
Indicates which component will be rendered on the screen. It affects the final data formatting in the generator but **must not** be used for making API requests.

#### `sort`
Defines which metric should be used for sorting the data. This is frequently used in the generator.

### Example Structures for Each Widget Type (Pinterest)
- **Datatable**
```json
{
  "reference_key": "pinterest:pins",
  "component": "datatable_v1",
  "metrics": [
    "description",
    "impression",
    "save",
    "created_at"
  ],
  "dimensions": [
    "board_pin"
  ],
  "sort": [
    "-impression"
  ]
}
```

- **Number**
```json
{
  "reference_key": "pinterest:saved",
  "component": "number_v1",
  "dimensions": [
    "user"
  ],
  "metrics": [
    "save"
  ]
}
```

- **Chart**
```json
{
  "reference_key": "pinterest:impressions_by_platform",
  "component": "chart_v1",
  "chart_type": "pie",
  "dimensions": [
      "app_type"
  ],
  "metrics": [
      "impression"
  ]
}
```

## Widget Configuration Files

### 1. Widget Definitions
Widget definitions (or skeletons) are stored in the widgets file.

**Steps:**
1. Inside `src/payloads/`, create a file named `network-name-widgets.json`.
2. Add the widget structures according to the component types listed earlier.

**Example:**

pinterest-widgets.json

```json
{
  "metrics": [
    {
      "reference_key": "pinterest:impressions_by_platform",
      "component": "chart_v1",
      "chart_type": "pie",
      "dimensions": [
          "app_type"
      ],
      "metrics": [
          "impression"
      ]
    },
    //... rest of widgets
  ]
}
```

### 2. Widget References
The widget skeleton alone does not define how it will be displayed or labeled in the front-end. The reference file serves this purpose.

**Steps:**
1. Inside `src/payloads/`, create a file named `network-name-references.json`.
2. Each widget in the widgets file must have a corresponding entry in the references file.

**Example:**

pinterest-references.json

```json
{
  "pinterest:impressions": {
    "title": "Impressions",
    "description": "The number of times your pins were viewed during the analysis period.",
    "tags": {
        "cost": "organic",
        "calculation": "Passed through the network",
        "value": "partial value",
        "history": "retroactive"
    }
  }
}
```

The key for each item in this associative array matches the `reference_key` from `pinterest-widgets.json`.

### Reference Structures for Each Widget Type
- **Reference for Datatable**
```json
"pinterest:total_boards": {
    "title": "Total boards",
    "description": "Total number of boards created during the analyzed period.",
    "tags": {
        "calculation": "calculated",
        "history": "retroactive",
        "cost": "organic",
        "value": "partial value"
    }
},
```

- **Reference for Chart**
```json
"pinterest:impressions_by_pin_format": {
    "title": "Impressions by format",
    "description": "The number of times your pins were seen by format.",
    "labels": ["Product", "Standard", "Story", "Video"],
    "tags": {
        "cost": "organic",
        "history": "retroactive",
        "calculation": "Passed through the network",
        "value": "total value"
    }
},
```
- **Reference for Number**
```json
"pinterest:impressions": {
  "title": "Impressions",
  "description": "The number of times your pins were viewed during the analysis period.",
  "tags": {
      "cost": "organic",
      "calculation": "Passed through the network",
      "value": "partial value",
      "history": "retroactive"
  }
}
```


## Network Setup
The setup allows metric editing, enabling users to customize tables, create custom charts, etc. Ideally, all metrics and dimensions added to widgets should also be included in the setup.

If a network allows table metrics to be removed or replaced with different ones, then the generator's architecture for that network is well-designed.

**Steps:**
1. Inside `src/payloads`, create a file named `network-name-setup.json`.

**Example:**
```json
{
  "metrics": {
    "impression": {
        "label": "Impressions"
    },
  },
  "dimensions": {
    "pin_format": {
      "label": "Format",
      "options": [
          "align_left"
      ]
    }
  }
}
```

2. Populate the `metrics` and `dimensions` arrays based on **all** metrics and dimensions added in the widgets file. This file consolidates all dimensions and metrics into one place.

Additionally, some options like `label`, `option`, and `formatting_function` need to be defined.

## Final Output
At the end of the process, we should have three separate files:

- **Widget Skeletons:** `network-name-widgets.json`
- **Widget References:** `network-name-references.json`
- **Standalone Metrics and Dimensions:** `network-name-setup.json`

