---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-09"

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

# Submitting a decision problem
{: #input}

You submit a decision problem to the {{site.data.keyword.tradeoffanalyticsshort}} service via its HTTP `POST dilemmas` method. You submit the objectives and options for the decision problem as a JSON `Problem` object in the body of the request. The following Unified Modeling Language (UML) diagram describes the `Problem` object.
{: shortdesc}

![Tradeoff Analytics Problem Object](images/TA-Problem-Object.png)

The following table describes the fields of the `Problem` object, all of which are required. The following sections describe the objects it comprises.

<table>
  <caption>Table 1. Fields of the <code>Problem</code> object</caption>
  <tr>
    <th style="text-align:left; width:20%">Field</th>
    <th style="text-align:center; width:20%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>subject</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      The name of the decision problem. The name typically reflects the
      category of the options.
    </td>
  </tr>
  <tr>
    <td><code>columns</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>Column</code>[ ]</td>
    <td>
      An array of possible objectives for the decision problem. The field
      specifies the columns for the tabular representation of the data.
      For more information, see <a href="#objectives">Specifying
        objectives</a>.
    </td>
  </tr>
  <tr>
    <td><code>options</code><br/><em>Required</em></td>
    <td style="text-align:center"><code>Option</code>[ ]</td>
    <td>
      An array of options for the decision problem. The field specifies
      the rows for the tabular representation of the data. For more
      information, see <a href="#options">Specifying options</a>.
    </td>
  </tr>
</table>

The following cURL example calls the `POST dilemmas` method with an input file named `problem.json`. The example sets the `Content-Type` header to `application/json`, which is required for all calls to the method. As part of the output, the service returns the parameters for the {{site.data.keyword.tradeoffanalyticsshort}} visualization; for more information, see [Generating a visualization](#visualization).

```bash
curl -X POST --user {username}:{password}
--header "Content-Type: application/json"
--data @problem.json
"https://gateway.watsonplatform.net/tradeoff-analytics/api/v1/dilemmas"
```
{: pre}

For examples of the objectives and options defined in the input file, see [Example objectives](#exampleObjectives) and [Example options](#exampleOptions).

## Generating a visualization
{: #visualization}

You can specify whether the service is to return just the resolution for the problem or both the resolution and the parameters for the {{site.data.keyword.tradeoffanalyticsshort}} visualization. The optional query parameter `generate_visualization` accepts a boolean value that specifies whether the service returns the visualization parameters. If you set the parameter to `true` or omit it, the service returns the visualization parameters.

Using the {{site.data.keyword.tradeoffanalyticsshort}} interface imposes certain requirements and limits on the size of the decision problem:

-   You can specify a maximum of 10 objectives for which the `is_objective` field is `true`. To generate visualization results, the field must be `true` for at least three objectives
-   The maximum number of options is calculated as the number of objectives multiplied by the number of best candidates. This value must be less than 2000.
-   To generate visualization results, at least three options must have a status of `FRONT` in the `Solution` object that is returned as part of the problem `Resolution` object. For more information, see [Understanding a problem resolution](/docs/services/tradeoff-analytics/output.html).

If you exceed either of the limits, the service returns an HTTP 413 error code ("Problem received is too large"). The service imposes no limits on the size of the decision problem when `generate_visualization` is `false`.

The following cURL example again calls the `POST dilemmas` method with an input file named `problem.json`. This example sets the `generate_visualization` query parameter to `false` to direct the service not to return the parameters for the {{site.data.keyword.tradeoffanalyticsshort}} visualization.

```bash
curl -X POST --user {username}:{password}
--header "Content-Type: application/json"
--data @problem.json
"https://gateway.watsonplatform.net/tradeoff-analytics/api/v1/dilemmas?generate_visualization=false"
```
{: pre}

The following sections describe which fields of the `Column` and `Option` objects are needed to define the decision problem and which are needed only if you plan to use the {{site.data.keyword.tradeoffanalyticsshort}} widget. For more information about using the {{site.data.keyword.tradeoffanalyticsshort}} client interface, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html). For information about coding with the client widget, see [Programming with the widget](/docs/services/tradeoff-analytics/client.html).

## Specifying objectives
{: #objectives}

The problem objectives specify the goals against which the service evaluates each option. You specify the objectives as an array of `Column` objects via the `columns` field of the `Problem` object. The following table describes the fields of the `Column` object that are needed to define the decision problem.

<table>
  <caption>Table 2. Fields of the <code>Column</code> object
    (definition)</caption>
  <tr>
    <th style="text-align:left; width:20%">Field</th>
    <th style="text-align:center; width:20%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>key</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      An identifier for the column that is unique among all columns
      for the problem.
    </td>
  </tr>
  <tr>
    <td><code>type</code><br/><em>Optional</em></td>
    <td style="text-align:center">String = <br/>
       "<code>text</code>" |<br/>
       "<code>numeric</code>" |<br/>
       "<code>datetime</code>" |<br/>
       "<code>categorical</code>"</td>
    <td>
      The type of the column:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>text</code> columns (the default) require a string.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>numeric</code> columns require a number literal (integer
          or double).
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>datetime</code> columns require a date and time string in
          full ISO 8601 format: <code>YYYY-MM-DDThh:mm:ss.sTZD</code>
          (for example, <code>2015-05-25T19:20:30.45+01:00</code> or
          <code>2015-05-26T00:00:00Z</code>)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>categorical</code> columns require a string that is one of
          the valid values specified by the <code>range</code> field for
          the column.
        </li>
      </ul>
      Use the <code>range</code> field to specify the range of valid
      values for an objective.
    </td>
  </tr>
  <tr>
    <td><code>goal</code><br/><em>Optional</em></td>
    <td style="text-align:center">String = <br/>
       "<code>min</code>" |<br/>
       "<code>max</code>"</td>
    <td>
      The direction for the column:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>min</code> indicates that the goal is to minimize the
          objective (for example, the price of a vehicle).
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>max</code> (the default) indicates that the goal is to
          maximize the objective (for example, the safety rating of a
          vehicle).
        </li>
      </ul>
      The <code>goal</code> is meaningful only for columns for which
      <code>is_objective</code> is <code>true</code>.
    </td>
  </tr>
  <tr>
    <td><code>is_objective</code><br/><em>Optional</em></td>
    <td style="text-align:center">Boolean</td>
    <td>
      An indication of whether the column is an objective for the problem.
      If <code>true</code>, the column contributes to the resolution; if
      <code>false</code> (the default), the column does not contribute
      to the resolution. A column of type <code>text</code> cannot be set
      to <code>true</code>. For restrictions that apply when you use the
      Tradeoff Analytics widget, see <a href="#visualization">Generating
        a visualization</a>.
    </td>
  </tr>
  <tr>
    <td><code>range</code><br/><em>Optional</em></td>
    <td style="text-align:center"><code>IRange</code></td>
    <td>
      The range of valid values for the column. Any option whose value
      for the column is outside of the specified range is marked as
      incomplete and is excluded from the resolution. By default, the range
      is calculated automatically from the minimum and maximum values that
      are provided in the data set for the column. For more information,
      see <a href="#ranges">Specifying ranges</a>.
    </td>
  </tr>
  <tr>
    <td><code>preference</code><br/><em>Optional</em></td>
    <td style="text-align:center">String[ ]</td>
    <td>
      For <code>categorical</code> columns, specifies an array of strings
      that is a preferred subset of the strings in the column's
      <code>range</code>. The order of the values in <code>preference</code>
      indicates the actual preference for the values in <code>range</code>.
      <br/><br/>
      The order of the values in <code>preference</code> is important.
      If <code>goal</code> is <code>min</code>, elements in the low
      position (at the front) of the array are favored over later
      elements; if <code>goal</code> is <code>max</code>, elements in
      the high position of the array are favored. By default, values
      are preferred according to their ordering in <code>range</code>
      and the direction indicated by <code>goal</code>.
    </td>
  </tr>
  <tr>
    <td><code>significant_gain</code><br/><em>Optional</em></td>
    <td style="text-align:center">Number</td>
    <td>
      A significant gain for the column in the range of 0 to 1. The
      value is a proportion of the complete range for the column. The
      field is relevant only for columns for which <code>is_objective</code>
      is <code>true</code>.
    </td>
  </tr>
  <tr>
    <td><code>significant_loss</code><br/><em>Optional</em></td>
    <td style="text-align:center">Number</td>
    <td>
      A significant loss for the column in the range of 0 to 1. The
      value is a proportion of the complete range for the column. The
      field is relevant only for columns for which <code>is_objective</code>
      is <code>true</code>.
    </td>
  </tr>
  <tr>
    <td><code>insignificant_loss</code><br/><em>Optional</em></td>
    <td style="text-align:center">Number</td>
    <td>
      An insignificant loss for the column in the range of 0 to 1. The
      value is a proportion of the complete range for the column. The
      field is relevant only for columns for which <code>is_objective</code>
      is <code>true</code>.
    </td>
  </tr>
</table>

The following table describes the fields of the `Column` object that are needed only if you plan to use the {{site.data.keyword.tradeoffanalyticsshort}} widget. The service does not use these fields to determine a problem resolution; it passes them as specified to the widget.

<table>
  <caption>Table 3. Fields of the <code>Column</code> object
    (visualization)</caption>
  <tr>
    <th style="text-align:left; width:20%">Field</th>
    <th style="text-align:center; width:20%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>format</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      For columns whose type is <code>numeric</code> or
      <code>datetime</code>, specifies how the value is to be displayed
      by the widget's interface. For more information, see
      <a href="#formats">Formatting numbers and dates</a>.
    </td>
  </tr>
  <tr>
    <td><code>full_name</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      A short description of the column. The <code>full_name</code> is
      displayed in the widget's interface. If you omit this field, the
      interface displays the column's <code>key</code> instead.
    </td>
  </tr>
  <tr>
    <td><code>description</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      A long description of the column. The <code>description</code> is
      displayed when the user examines the column more closely in the
      widget's interface.
    </td>
  </tr>
</table>

### Specifying ranges
{: #ranges}

You can specify the range of valid values for a column via the `range` field of the `Column` object. The service excludes from the resolution any option whose value for the column is outside of the specified range, indicating that it is incomplete. In addition, the {{site.data.keyword.tradeoffanalyticsshort}} interface applies the specified range when it displays the problem resolution, so you can use the range to enforce a scale in the interface.

Specifying a range is useful, for example, when an objective has a natural range that you want to enforce. For instance, valid percentages lie between 0 and 100. You can specify a range to enforce this constraint, and the widget's interface also reflects the specified range.

The `range` field accepts an abstract object of type `IRange`, which in turn accepts different types of ranges depending on the type of the column:

-   For `numeric` columns, you specify a `ValueRange` that indicates the `high` and `low`, or maximum and minimum, valid values for the column's range. You specify the values as integers or doubles. The following example specifies a range of 50 to 100 for a `numeric` objective:

    ```javascript
    "range": {
      "low": 50,
      "high": 100
    }
    ```
    {: codeblock}

-   For `datetime` columns, you specify a `DateRange` that indicates the `high` and `low`, or latest and earliest, valid dates for the column's range. You specify the values as strings in full ISO 8601 format. The following example specifies a range from July 1, 2015, to December 31, 2015, for a `datetime` objective:

    ```javascript
    "range": {
      "low": "2015-07-01T00:00:00Z",
      "high": "2015-12-31T23:59:59Z"
    }
    ```
    {: codeblock}

-   For `categorical` columns, you specify a `CategoricalRange` that lists the `keys` that are valid for the column. By default, values are preferred in the order in which you list them in the array. The column's `goal` field indicates the order of preference. If `goal` is `min`, elements in the low position (at the front) of the array are preferred; if `goal` is `max`, elements in the high position are preferred. You can also use the `preference` field to list a subset of the elements in the range in their preferred order. The following example specifies a range of four acceptable values for a `categorical` objective:

    ```javascript
    "range": [
      "Apple",
      "HTC",
      "Samsung",
      "Sony"
    ]
    ```
    {: codeblock}

You cannot specify a range for a column of type `text`.

### Formatting numbers and dates
{: #formats}

For a column of type `numeric` or `datetime`, you can use the `format` field of the `Column` object to specify how the interface of the {{site.data.keyword.tradeoffanalyticsshort}} widget is to format values specified by options for the column. The `format` field accepts different types of specifications for the two types of columns. In both cases, the {{site.data.keyword.tradeoffanalyticsshort}} interface interprets the pattern according to the locale of the user's browser.

**For `numeric` columns**, you can specify the following:

-   The number of decimal places in the format `"number: n"`, where *n* is a positive integer value that indicates the number of decimal places.
-   A currency symbol in the format `"currency: 'symbol' : n"`, where *symbol* is the currency symbol and *n* is a positive integer value that indicates the number of decimal places.
-   A prefix in the format `"taPrefix: 'symbol'"`, where *symbol* is the prefix string.
-   A suffix in the format `"taSuffix: 'symbol'"`, where *symbol* is the suffix string.

You can combine formatters with a `|` (vertical bar). The following table shows examples of the `numeric` formatters.

<table>
  <caption>Table 4. Examples of <code>numeric</code> formatters</caption>
  <tr>
    <th style="text-align:left; width:40%">Pattern</th>
    <th style="text-align: center; width:25%">Input</th>
    <th style="text-align: center; width:15%">Locale</th>
    <th style="text-align: center; width:20%">Result</th>
  </tr>
  <tr>
    <td>
      <code>"format": "number: 2"</code>
    </td>
    <td style="text-align: center">
      239.9
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      239.90
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      French
    </td>
    <td style="text-align: center">
      239,90
    </td>
  </tr>
  <tr>
    <td>
      <code>"format": "number: 1"</code>
    </td>
    <td style="text-align: center">
      150.92
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      150,9
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      French
    </td>
    <td style="text-align: center">
      150,9
    </td>
  </tr>
  <tr>
    <td>
      <code>"format": "currency: 'USD$' : 2"</code>
    </td>
    <td style="text-align: center">
      199
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      USD$199.00
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      French
    </td>
    <td style="text-align: center">
      199,00 USD$
    </td>
  </tr>
  <tr>
    <td>
      <code>"format": "taPrefix: 'g'"</code>
    </td>
    <td style="text-align: center">
      199
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      g199
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      French
    </td>
    <td style="text-align: center">
      g199
    </td>
  </tr>
  <tr>
    <td>
      <code>"format": "taSuffix: 'g'"</code>
    </td>
    <td style="text-align: center">
      199
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      199g
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      French
    </td>
    <td style="text-align: center">
      199g
    </td>
  </tr>
  <tr>
    <td>
      <code>"format": "number: 2 | taSuffix: 'g'"</code>
    </td>
    <td style="text-align: center">
      199
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      199.00g
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      French
    </td>
    <td style="text-align: center">
      199,00g
    </td>
  </tr>
</table>

**For `datetime` columns**, you can specify how dates and times are to be presented in the format `"date: 'format'"`, where *format* indicates the date and time pattern. The following table shows examples of the `datetime` formatter.

<table>
  <caption>Table 5. Examples of the <code>datetime</code> formatter</caption>
  <tr>
    <th style="text-align: left; width:40%">Pattern</th>
    <th style="text-align: center; width:25%">Input</th>
    <th style="text-align: center; width:15%">Locale</th>
    <th style="text-align: center; width:20%">Result</th>
  </tr>
  <tr>
    <td>
      <code>"format": "date: 'MMM dd, yyyy'"</code>
    </td>
    <td style="text-align: center">
      2015-05-07T09:23:31Z
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      May 7, 2015
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      French
    </td>
    <td style="text-align: center">
      mai 7, 2015
    </td>
  </tr>
  <tr>
    <td>
      <code>"format": "date: 'h:m:s a'"</code>
    </td>
    <td style="text-align: center">
      2015-05-07T09:23:31Z
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      9:23:31 AM
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      Chinese
    </td>
    <td style="text-align: center">
      9:23:31 &#19978;&#21320;
    </td>
  </tr>
  <tr>
    <td>
      <code>"format": "date: 'MMM dd, yyyy h:m:s a'"</code>
    </td>
    <td style="text-align: center">
      2015-05-07T09:23:31Z
    </td>
    <td style="text-align: center">
      English
    </td>
    <td style="text-align: center">
      May 7, 2015 9:23:31 AM
    </td>
  </tr>
  <tr>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="border-bottom: 1px solid #eee;"></td>
    <td style="text-align: center">
      Spanish
    </td>
    <td style="text-align: center">
      may 7, 2015 9:23:31 am
    </td>
  </tr>
</table>

For more information about specifying `number`, `currency`, and `date` formatter patterns, see the descriptions of the corresponding filter components in the [AngularJS documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.angularjs.org/api/ng/filter){: new_window}.

### Example objectives
{: #exampleObjectives}

The following example shows the objectives that are defined in the file named `problem.json`. The input begins with a `subject` field that describes the decision problem, in this case a problem called `phones`. The `columns` field provides an array of four objectives for the problem: `price`, `weight`, `brand`, and `rDate` (release date). Only the first three are defined as objectives for the problem by setting the `is_objective` field to `true`; the fourth is informational only.

The three columns that are objectives set the `goal` field to `min`. For `price` and `weight`, this defines a preference for lower values. For `brand`, this indicates the order in which values listed in the `preference` field are preferred: Values at the start of the list are preferred over values at the end.

The input defines allowable ranges of values for the `price` and `brand` columns. It also provides formats for the presentation of the `price`, `wieght`, and `rDate` columns when they are displayed by the {{site.data.keyword.tradeoffanalyticsshort}} client widget.

```javascript
{
  "subject": "phones",
  "columns": [
    {
      "key": "price",
      "type": "numeric",
      "goal": "min",
      "is_objective": true,
      "full_name": "Price",
      "range": {
        "low": 0,
        "high": 400
      },
      "format": "number:2"
    },
    {
      "key": "weight",
      "type": "numeric",
      "goal": "min",
      "is_objective": true,
      "full_name": "Weight",
      "format": "number:0"
    },
    {
      "key": "brand",
      "type": "categorical",
      "goal": "min",
      "is_objective": true,
      "full_name": "Brand",
      "range": [
        "Apple",
        "HTC",
        "Samsung",
        "Sony"
      ],
      "preference": [
        "Samsung",
        "Apple",
        "HTC"
      ]
    },
    {
      "key": "rDate",
      "type": "datetime",
      "goal": "max",
      "full_name": "Release Date",
      "format": "date: 'MMM dd, yyyy'"
    }
  ],
  . . .
}
```
{: codeblock}

The remainder of the file, which specifies the options for the decision problem, is shown in [Example options](#exampleOptions).

## Specifying options
{: #options}

The problem options define the candidates the service is to evaluate against the specified objectives. The objectives define the goals of the problem; the options meet those objectives to varying extents. You specify options as an array of `Option` objects via the `options` field of the `Problem` object. Each option must specify a valid value for each column whose `is_objective` field is `true`. The following table describes the fields of the `Option` object that are needed to define the decision problem.

<table>
  <caption>Table 6. Fields of the <code>Option</code> object
    (definition)</caption>
  <tr>
    <th style="text-align:left; width:20%">Field</th>
    <th style="text-align:center; width:30%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>key</code><br/><em>Required</em></td>
    <td style="text-align:center">String</td>
    <td>
      An identifier for the option that is unique among all options for the
      problem.
    </td>
  </tr>
  <tr>
    <td><code>values</code><br/><em>Required</em></td>
    <td style="text-align:center">{String: Number | String, ...}</td>
    <td>
      A map of key-value pairs that specifies a value for each column
      in the format<br/><br/>
      <code>"values": { "key1": value1, "key2": value2 }</code><br/><br/>
      A value must be of the <code>type</code> defined for its column.
      An option that fails to specify a value for a column for which
      <code>is_objective</code> is <code>true</code> is marked as
      <code>INCOMPLETE</code> and is excluded from the resolution.
    </td>
  </tr>
</table>

The following table describes the fields of the `Option` object that are needed only if you plan to use the {{site.data.keyword.tradeoffanalyticsshort}} widget. The service does not use these fields to determine a problem resolution; it passes them as specified to the widget.

<table>
  <caption>Table 7. Fields of the <code>Option</code> object
    (visualization)</caption>
  <tr>
    <th style="text-align:left; width:20%">Field</th>
    <th style="text-align:center; width:20%">Data type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>name</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      The name of the option.
    </td>
  </tr>
  <tr>
    <td><code>description_html</code><br/><em>Optional</em></td>
    <td style="text-align:center">String</td>
    <td>
      A description of the option in HTML format. The description is
      displayed when the user selects the option from the widget's
      interface.
    </td>
  </tr>
  <tr>
    <td><code>app_data</code><br/><em>Optional</em></td>
    <td style="text-align:center">{String: String, ...}</td>
    <td>
      A map of key-value pairs in which the application can pass
      application-specific information in the format<br/><br/>
      <code>"app_data": { "key1": value1, "key2": value2 }</code><br/><br/>
      The service carries the information but does not use it.
    </td>
  </tr>
</table>

### Example options
{: #exampleOptions}

The following example shows the options that are defined in the file named `problem.json`. The `options` field provides an array of seven phones that are to be considered for the problem. Each option is identified by a unique `key`, and each provides `values` for the problem's four objectives. The service compares the options against both the objectives and each other to determine the top candidates from among the seven. This specification completes the description of the input file begun in [Example objectives](#exampleObjectives).

```javascript
{
  . . .
  "options": [
    {
      "key": "1",
      "name": "Samsung Galaxy S4",
      "values": {
        "price": 249,
        "weight": 130,
        "brand": "Samsung",
        "rDate": "2013-04-29T00:00:00Z"
      }
    },
    {
      "key": "2",
      "name": "Apple iPhone 5",
      "values": {
        "price": 349,
        "weight": 112,
        "brand": "Apple",
        "rDate": "2012-09-21T00:00:00Z"
      }
    },
    {
      "key": "3",
      "name": "HTC One",
      "values": {
        "price": 299,
        "weight": 112,
        "brand": "HTC",
        "rDate": "2013-03-01T00:00:00Z"
      }
    },
    {
      "key": "4",
      "name": "Samsung Galaxy S5",
      "values": {
        "price": 349,
        "weight": 135,
        "brand": "Samsung",
        "rDate": "2014-04-29T00:00:00Z"
      }
    },
    {
      "key": "5",
      "name": "Apple iPhone 6",
      "values": {
        "price": 399,
        "weight": 118,
        "brand": "Apple",
        "rDate": "2013-09-21T00:00:00Z"
      }
    },
    {
      "key": "6",
      "name": "Apple iPhone 7",
      "values": {
        "price": 499,
        "weight": 118,
        "brand": "Apple",
        "rDate": "2014-09-21T00:00:00Z"
      }
    },
    {
      "key": "7",
      "name": "Sony Xperia",
      "values": {
        "price": 199,
        "weight": 120,
        "brand": "Sony",
        "rDate": "2014-08-21T00:00:00Z"
      }
    }
  ]
}
```
{: codeblock}

## Requesting preferable options
{: #preferable}

By default, the service identifies the best candidates from among the available options. It does this by considering each option against both the objectives and the other options. But after excluding those options that are clearly inferior to others based on their values for the objectives, the subset of options that remains can still be large.

You can request that the service perform further analysis of the best candidates to identify those most likely to satisfy the greatest number of users. To identify this smaller subset of preferred solutions, the service relies on a probabilistic model that compares the candidates' values for the objectives and applies a probability distribution of general user preferences. It performs this analysis without relying on users' personal data.

The service usually identifies no more than five preferred solutions; it typically selects just three or fewer solutions. The service also provides a confidence score that indicates the percentage of users who are likely to choose one of the preferred solutions. This information further encourages an informed decision, one that leads to selecting the best available solution to the decision problem.

To request the set of preferred solutions, set the optional query parameter `find_preferable_options` to `true` in a call to the `POST dilemmas` method, as in the following cURL example. The example again uses the `problem.json` file and sets the `Content-Type` header to `application/json`. Because the example omits the `generate_visualization` query parameter, the service returns the parameters for the map visualization.

```bash
curl -X POST --user {username}:{password}
--header "Content-Type: application/json"
--data @problem.json
"https://gateway.watsonplatform.net/tradeoff-analytics/api/v1/dilemmas?find_preferable_options=true"
```
{: pre}

For an example of the results of this request and a discussion of the additional information it returns, see [Example preferable solutions](/docs/services/tradeoff-analytics/output.html#examplePreferable).
