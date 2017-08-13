---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-13"

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

# Decision problem
{: #problem}

1.  <span style="color:#003F69">Is there a way to rank the objectives for a decision problem?</span>

    No, all objectives are equally important. However, you can use filters to set thresholds on objectives' values, modifying the objectives to suit your preferences. For more information, see [Filtering objectives](/docs/services/tradeoff-analytics/visualization.html#filtering).

1.  <span style="color:#003F69">Does the service support objectives with non-numeric values?</span>

    Yes. The service supports four types of objectives: `numeric`, `datetime`, `categorical`, and `text`. For information about specifying types of objectives, see [Specifying objectives](/docs/services/tradeoff-analytics/input.html#objectives); for information about working with objectives in a visualization, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html).

1.  <span style="color:#003F69">Is there a limit on the size of the input data set?</span>

    If the `generate_visualization` query parameter of the `POST /v1/dilemmas` method is set to `true`, the service imposes the limits described in [Generating a visualization](/docs/services/tradeoff-analytics/input.html#visualization). If the parameter is `false`, no limits apply.

1.  <span style="color:#003F69">Does the service accept input in a format other than JSON (for example, online data)?</span>

    Currently, the service supports input represented only in JSON format, but the input can be generated dynamically. For instance, assume an application in the retail domain has an initial set of objectives. The application can access a catalog of products and generate one option per product in the catalog. The application is responsible for defining the objectives correctly and for any transformation applied to the objective values (for example, converting units of measure).

    The service offers a [CSV importer tool ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://tradeoff-analytics-csvtojson.mybluemix.net/){: new_window} that lets you import data from your own Microsoft&reg; Excel file by converting it to JSON. The tool guides you through the process of uploading the data into the service.
