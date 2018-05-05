---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Understanding a problem resolution
{: #output}

When you submit a decision problem to the {{site.data.keyword.tradeoffanalyticsshort}} service, the service returns its results as a JSON `Dilemma` object that includes both the original `Problem` and its `Resolution`. The following Unified Modeling Language (UML) diagram describes the `Dilemma` object.
{: shortdesc}

![Tradeoff Analytics Dilemma Object](images/TA-Dilemma-Object.png)

The following table describes the fields of the `Dilemma` object, both of which are always included in the response.

<table>
  <caption>Table 1. Fields of the <code>Dilemma</code> object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:30%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>problem</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>Problem</code></td>
    <td>
      The <code>Problem</code> object submitted in the call to the
      <code>dilemmas</code> method of the service. For more information,
      see <a href="/docs/services/tradeoff-analytics/input.html">Submitting
      a decision problem</a>.
    </td>
  </tr>
  <tr>
    <td><code>resolution</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>Resolution</code></td>
    <td>
      The resolution of the decision problem sent to the service. For
      more information, see <a href="#resolution">The Resolution object</a>.
    </td>
  </tr>
</table>

To see the `Resolution` object that the service returns for the `problem.json` file described in [Specifying a decision problem](/docs/services/tradeoff-analytics/input.html), see [Example solutions](#exampleSolutions), [Example preferable solutions](#examplePreferable), and [Example map visualization](#exampleMap).

## The Resolution object
{: #resolution}

The `Resolution` object contains the service's response to the decision problem. The following table describes the fields of the `Resolution` object.

<table>
  <caption>Table 2. Fields of the <code>Resolution</code> object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:30%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>solutions</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>Solution</code>[ ]</td>
    <td>
      An array of <code>Solution</code> objects that contains
      the analytical data prepared by the service for each option of the
      decision problem. For more information, see <a href="#solution">The
        Solution object</a>.
    </td>
  </tr>
  <tr>
    <td><code>preferable_solutions</code><br/><em>Optional</em></td>
    <td style="text-align:center"><code>PreferableSolutions</code></td>
    <td>
      A <code>PreferableSolutions</code> object that usually identifies
      no more than five of the best candidate solutions that are likely
      to satisfy the greatest number of users. The resolution includes
      this field only if the query parameter
      <code>find_preferable_options</code> is <code>true</code>. For
      more information, see
      <a href="#preferable">The PreferableSolutions object</a>.
    </td>
  </tr>
  <tr>
    <td><code>map</code><br/><em>Optional</em></td>
    <td style="text-align:center"><code>Map</code></td>
    <td>
      The two-dimensional positioning of each option on the map polygon
      displayed by the widget's interface. The resolution includes this
      field only if the query parameter <code>generate_visualization</code>
      is <code>true</code>, the <code>is_objective</code> field is
      <code>true</code> for at least three columns, and at least three
      options have a status of <code>FRONT</code> in the problem resolution.
      For more information, see <a href="#map">The Map object</a>.
    </td>
  </tr>
</table>

## The Solution object
{: #solution}

The service returns an array of `Solution` objects that always includes an element for each option of the decision problem. The resolution contains the results of the service's analysis of each option in the context of both the objectives and the other options. The information includes the option's status and its possible relationship to other options. The following table describes the fields of the `Solution` object.

<table>
  <caption>Table 3. Fields of the <code>Solution</code> object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:35%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>solution_ref</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      The <code>key</code> that uniquely identifies the option in the
      decision problem.
    </td>
  </tr>
  <tr>
    <td><code>status</code><br/><em>Required</em></td>
    <td style="text-align:center">String = <br/>
       "<code>FRONT</code>" |<br/>
       "<code>EXCLUDED</code>" |<br/>
       "<code>INCOMPLETE</code>" |<br/>
       "<code>DOES_NOT_MEET_PREFERENCE</code>"</td>
    <td>
      The status of the option for the problem resolution:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>FRONT</code> indicates that the option is included among
          the best candidates for the problem.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>EXCLUDED</code> indicates that another option is
          strictly better than the option.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>INCOMPLETE</code> indicates that either the option's
          specification does not include a value for one of the columns
          or its value for one of the columns lies outside the range
          specified for the column. Only a column whose
          <code>is_objective</code> field is set to <code>true</code>
          can generate this status.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>DOES_NOT_MEET_PREFERENCE</code> indicates that the
          the option specifies a value for a categorical column that
          is not included in the column's <code>preference</code>.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td><code>excluded_by</code><br/><em>Optional</em></td>
    <td style="text-align:center"><code>ExcludedBy</code>[ ]</td>
    <td>
      If the status of the option is <code>EXCLUDED</code>, an array of
      <code>ExcludedBy</code> objects that lists each of the superior
      options that excluded the current option and describes each value
      that was strictly better than the current option's value. For more
      information, see <a href="#excludedBy">The ExcludedBy object</a>.
    </td>
  </tr>
  <tr>
    <td><code>status_cause</code><br/><em>Optional</em></td>
    <td style="text-align:center"><code>StatusCause</code></td>
    <td>
      If the status of the option is <code>INCOMPLETE</code> or
      <code>DOES_NOT_MEET_PREFERENCE</code>, a <code>StatusCause</code>
      object that provides more information about the cause of the status.
      For more information, see <a href="#StatusCause">The StatusCause
        object</a>.
    </td>
  </tr>
  <tr>
    <td><code>shadows</code><br/><em>Optional</em></td>
    <td style="text-align:center">String[ ]</td>
    <td>
      An array of references to the <code>key</code>s of solutions that
      are shadowed by this solution.
<!--
      This option is strictly better than each option that it shadows,
      each of which has a <code>status</code> of <code>EXCLUDED</code>.
-->
    </td>
  </tr>
  <tr>
    <td><code>shadow_me</code><br/><em>Optional</em></td>
    <td style="text-align:center">String[ ]</td>
    <td>
      An array of references to the <code>key</code>s of solutions that
      shadow this solution.
<!--
      Each option that shadows this option is strictly better than this
      option. An option that has a <code>shadow_me</code> field has a
      status of <code>EXCLUDED</code>.
-->
    </td>
  </tr>
</table>

### The ExcludedBy object
{: #excludedBy}

If the status of an option is `EXCLUDED`, the `excluded_by` field of the `Solution` object for the option specifies an array of `ExcludedBy` objects that provides information about the options that were strictly better. The following table describes the fields of the `ExcludedBy` object.

<table>
  <caption>Table 4. Fields of the <code>ExcludedBy</code> object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:30%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>solution_ref</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      The <code>key</code> that uniquely identifies the option that was
      strictly better than the current option.
    </td>
  </tr>
  <tr>
    <td><code>objectives</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>Objective</code>[ ]</td>
    <td>
      An array of <code>Objective</code> objects that describes each
      value of the superior option that was better than the current option.
    </td>
  </tr>
</table>

An `Objective` object provided in the `objectives` field includes the following fields.

<table>
  <caption>Table 5. Fields of the <code>Objective</code> object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:30%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>key</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      The <code>key</code> that uniquely identifies the column
      (objective) of the superior option whose value was better than
      the corresponding value of the current option.
    </td>
  </tr>
  <tr>
    <td><code>difference</code><br/><em>Required</em></td>
    <td style="text-align:center">Number</td>
    <td>
      The difference between the values of the superior option and the
      current option for the column (objective). The difference can be
      an integer or a double depending on the column definition.
    </td>
  </tr>
  <tr>
    <td><code>text</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      For <code>categorical</code> columns, a description of the
      difference between the two values as a preference. For
      <code>datetime</code> columns, a description of the difference
      between the two values as a number of days, hours, and minutes,
      as needed.
    </td>
  </tr>
</table>

### The StatusCause object
{: #StatusCause}

If the status of an option is `INCOMPLETE` or `DOES_NOT_MEET_preFERENCE`, the `status_cause` field of the `Solution` object for the option specifies a `StatusCause` object that provides more information about the option's status. The following table describes the fields of the `StatusCause` object.

<table>
  <caption>Table 6. Fields of the <code>StatusCause</code> object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:35%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>message</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      A description in English of the cause for the option's status.
    </td>
  </tr>
  <tr>
    <td><code>tokens</code><br/><em>Required</em></td>
    <td style="text-align:center">String[ ]</td>
    <td>
      An array of values used to describe the cause for the option's
      status. The strings appear in the <code>message</code> field.
    </td>
  </tr>
  <tr>
    <td><code>error_code</code><br/><em>Required</em></td>
    <td style="text-align:center">String = <br/>
       "<code>MISSING_OBJECTIVE_VALUE</code>" |<br/>
       "<code>RANGE_MISMATCH</code>" |<br/>
       "<code>DOES_NOT_MEET_PREFERENCE</code>"</td>
    <td>
      An error code that specifies the cause of the option's
      status:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>MISSING_OBJECTIVE_VALUE</code> indicates that a column
          for which the <code>is_objective</code> field is <code>true</code>
          is absent from the option's specification.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>RANGE_MISMATCH</code> indicates that the option's
          specification defines a value that is outside of the range
          specified for an objective.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>DOES_NOT_MEET_PREFERENCE</code> indicates that a
          categorical column value for the option is not included among
          the preferences for that column.
        </li>
      </ul>
    </td>
  </tr>
</table>

### Example solutions
{: #exampleSolutions}

The service returns a JSON `Dilemma` object that includes a `Problem` object that duplicates the decision problem exactly as it was submitted by the `problem.json` file. The `Problem` object includes the `subject` field and the `Columns` and `Options` objects in their entirety. For a complete description of the contents of the file, see [Example objectives](/docs/services/tradeoff-analytics/input.html#exampleObjectives) and [Example options](/docs/services/tradeoff-analytics/input.html#exampleOptions).

The `Dilemma` object then includes a `Resolution` object that provides the results of the service's evaluation of the seven phones defined by the decision problem. The `solutions` field provides an array of `Solution` objects with results for each option. For each element of the array, the `solution_ref` field identifies the key of an option, and the `status` field indicates the option's disposition in the resolution:

-   The first three options have a status of `FRONT` to indicate that they are among the best candidates.
-   The next two options have a status of `EXCLUDED`. This means that one or more clearly superior choices exist for each option. For both options, the output includes an `excluded_by` field that lists the superior option and describes the values of that option that were better than those of the option in question. The field is an array that can list multiple superior options if the service finds more than one.
-   Option `6` has a status of `INCOMPLETE`. The `status_cause` field indicates that the option has a `RANGE_MISMATCH` with the `price` column: Its price exceeds the maximum range of that objective.
-   Option `7` has a status of `DOES_NOT_MEET_preFERENCE`. The `status_cause` field indicates that the option has a value (`Sony`) that is not included among the preferred list of brands.

```javascript
{
  "problem": {
    "columns": [
      . . .
    ],
    "subject": "phones",
    "options": [
      . . .
    ]
  },
  "resolution": {
    "solutions": [
      {
        "solution_ref": "1",
        "status": "FRONT"
      },
      {
        "solution_ref": "2",
        "status": "FRONT"
      },
      {
        "solution_ref": "3",
        "status": "FRONT"
      },
      {
        "solution_ref": "4",
        "status": "EXCLUDED",
        "excluded_by": [
          {
            "solution_ref": "1",
            "objectives": [
              {
                "key": "price",
                "difference": 100
              },
              {
                "key": "weight",
                "difference": 5
              }
            ]
          }
        ]
      },
      {
        "solution_ref": "5",
        "status": "EXCLUDED",
        "excluded_by": [
          {
            "solution_ref": "2",
            "objectives": [
              {
                "key": "price",
                "difference": 50
              },
              {
                "key": "weight",
                "difference": 6
              }
            ]
          }
        ]
      },
      {
        "solution_ref": "6",
        "status": "INCOMPLETE",
        "status_cause": {
          "message": "A column of a option is out of range. Option \"6\" has a value in column \"price\" which is:\"499\" while the column range\" is: [0.0,400.0]\"",
          "error_code": "RANGE_MISMATCH",
          "tokens": [
            "price",
            "499",
            "[0.0,400.0]"
          ]
        }
      },
      {
        "solution_ref": "7",
        "status": "DOES_NOT_MEET_preFERENCE",
        "status_cause": {
          "message": "Option \"7\" has a value that does not meet preference for column \"brand\"",
          "error_code": "DOES_NOT_MEET_preFERENCE",
          "tokens": [
            "brand"
          ]
        }
      }
    ]
  }
}
```
{: codeblock}

For examples of the preferred solutions and the map visualization that the service can return, see [Example preferable solutions](#examplePreferable) and [Example map visualization](#exampleMap).

## The PreferableSolutions object
{: #preferable}

The `PreferableSolutions` object identifies the subset of best candidates that are likely to satisfy the greatest number of users. The reduced list of preferred solutions can make it easier to select the best possible candidate. The `Resolution` object includes the `PreferableSolutions` object only if the query parameter `find_preferable_options` is `true`. The following table describes the fields of the object.

<table>
  <caption>Table 7. Fields of the <code>PreferableSolutions</code>
    object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:30%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>solution_refs</code><br/><em>Required</em></td>
    <td style="text-align:center">String[ ]</td>
    <td>
      An array of <code>key</code>s that identifies the subset of
      preferred solutions from among the best candidates of the
      decision problem. The service selects the subset from the
      solutions that have a status of <code>FRONT</code>.
    </td>
  </tr>
  <tr>
    <td><code>score</code><br/><em>Required</em></td>
    <td style="text-align:center">Number</td>
    <td>
      A confidence score that indicates the percentage of users that
      the service believes will choose one of the preferred solutions
      as opposed to one of the other best candidates. The higher the
      percentage, the greater the service's confidence in its selections.
    </td>
  </tr>
</table>

### Example preferable solutions
{: #examplePreferable}

If the service returns preferable solutions, the `Resolution` object includes the `PreferableSolutions` object. The following example shows the object that the service returns for the decision problem submitted by the `problem.json` file. This output continues the description of the service's response begun in [Example solutions](#exampleSolutions).

The response lists the keys for two of the problem's seven options, three of which were identified as best candidates with a status of `FRONT`. The response includes a score of 94 percent, indicating a high degree of confidence in the preferred solutions.

```javascript
{
  "problem": {
    "columns": [
      . . .
    ],
    "subject": "phones",
    "options": [
      . . .
    ]
  },
  "resolution": {
    "solutions": [
      . . .
    ],
    "preferable_solutions": {
      "solution_refs": [
        "1",
        "2"
      ],
      "score": 0.94
    }
  }
}
```
{: codeblock}

The following three candidates have a status of `FRONT`:

<table>
  <caption>Table 8. Example candidates with <code>FRONT</code> status</caption>
  <tr>
    <th style="width:12%; text-align:center">Key</th>
    <th style="width:40%">Option</th>
    <th style="width:16%; text-align:center">Price (<code>min</code>)</th>
    <th style="width:16%; text-align:center">Weight (<code>min</code>)</th>
    <th style="text-align:center">Brand (<code>min</code>)</th>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Samsung Galaxy S4</td>
    <td style="text-align:center">249</td>
    <td style="text-align:center">130</td>
    <td style="text-align:center">Samsung</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Apple iPhone 5</td>
    <td style="text-align:center">349</td>
    <td style="text-align:center">112</td>
    <td style="text-align:center">Apple</td>
  </tr>
  <tr>
    <td style="text-align:center">3</td>
    <td>HTC One</td>
    <td style="text-align:center">299</td>
    <td style="text-align:center">112</td>
    <td style="text-align:center">HTC</td>
  </tr>
</table>

None of the three is strictly better than the others: Option `1` has a lower price than the others, while options `2` and `3` have a lower weight than option `1`. But the brand of option `1` is also the most preferred of those listed for the decision problem, giving it an edge over the other two options (in the map visualization, more of its glyph would be filled). But its higher weight compelled the service to consider additional solutions, and the service's algorithm chose option `2`. The algorithm does not and cannot consider the meaning of the individual objectives, but its probabilistic model concluded that options `1` and `2` are most likely to satisfy the needs of a majority of users.

For an example of the map visualization that the service can return, see [Example map visualization](#exampleMap).

## The Map object
{: #map}

The `Map` object specifies the two-dimensional positioning of each option on the {{site.data.keyword.tradeoffanalyticsshort}} map visualization. The {{site.data.keyword.tradeoffanalyticsshort}} widget consumes the fields of this object to render the visualization. The `Resolution` object includes the `Map` object only if all of the following are true:

-   The query parameter `generate_visualization` is `true` (the default value if the parameter is omitted).
-   The `is_objective` field is `true` for at least three columns.
-   At least three options have a status of `FRONT` in the problem resolution.

The following UML diagram depicts the high-level fields of the `Map` object. Additional low-level fields of the object are not described. All of the object's fields are related to the placement of nodes and cells on the map visualization.

![Tradeoff Analytics Map Object](images/TA-Map-Object.png)

The following table describes these fields of the `Map` object.

<table>
  <caption>Table 9. Fields of the <code>Map</code> object</caption>
  <tr>
    <th style="text-align:left; width:25%">Field</th>
    <th style="text-align:center; width:30%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>anchors</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>Anchor</code>[ ]</td>
    <td>
      An array of <code>Anchor</code> objects that represent the
      vertices for the objectives and their positions on the map
      visualization.
    </td>
  </tr>
  <tr>
    <td><code>nodes</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>MapNode</code>[ ]</td>
    <td>
      An array of <code>MapNode</code> objects for the cells on the
      map visualization. Each cell in the array includes coordinates
      that describe the position on the map of the glyphs for one or
      more listed options, which are identified by their keys.
    </td>
  </tr>
</table>

### Example map visualization
{: #exampleMap}

If the service generates the map visualization, the `Resolution` object includes the `Map` object. This object provides the parameters that are consumed by the {{site.data.keyword.tradeoffanalyticsshort}} client widget to display the visualization. Its fields have meaning only for the widget; you do not need to understand them to use the visualization interface. The following example shows the object that the service returns for the decision problem submitted by the `problem.json` file. This completes the description of the service's response from [Example solutions](#exampleSolutions) and [Example preferable solutions](#examplePreferable).

```javascript
{
  "problem": {
    "columns": [
      . . .
    ],
    "subject": "phones",
    "options": [
      . . .
    ]
  },
  "resolution": {
    "solutions": [
      . . .
    ],
    "map": {
      "nodes": [
        {
          "coordinates": {
            "x": 3,
            "y": 0
          },
          "solution_refs": [
            "1"
          ]
        },
        {
          "coordinates": {
            "x": 2,
            "y": 3.4641016151378
          },
          "solution_refs": [
            "3"
          ]
        },
        {
          "coordinates": {
            "x": 4,
            "y": 3.4641016151378
          },
          "solution_refs": [
            "2"
          ]
        }
      ],
      "anchors": [
        {
          "name": "price",
          "position": {
            "x": 0,
            "y": 0
          }
        },
        {
          "name": "weight",
          "position": {
            "x": 3,
            "y": 5.1961524227066
          }
        },
        {
          "name": "brand",
          "position": {
            "x": 6,
            "y": 0
          }
        }
      ],
      "version": "APIV1-ANN",
      "config": {
        "params": {
          "rInit": 4.8151012450415,
          "rFinish": 1,
          "seed": 20008,
          "rAnchor": 1.5,
          "alpha_init": 0.75,
          "map_size": 27,
          "training_period": 100,
          "anchor_epoch": 3
        },
        "drivers": {
          "r_fin": 1,
          "r_init": 1.2,
          "r_anchor_init": 1.8,
          "training_length": 100,
          "max_map_size": 200,
          "alpha_init": 0.75,
          "training_anchors": 1,
          "data_multiplier": 9
        }
      },
      "metrics": {
        "mqe": "NaN",
        "tau": "NaN",
        "somers": "NaN",
        "kappa": 1,
        "kappa_delta": "NaN",
        "weighted_kappa": "NaN",
        "final_kappa": 1
      }
    }
  }
}
```
{: codeblock}
