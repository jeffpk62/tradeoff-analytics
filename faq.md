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

# FAQs
{: #faq}

As you work with the {{site.data.keyword.tradeoffanalyticsshort}} service, you may have questions about specific aspects of the service, its use, and its results. Some common questions and their answers follow. The questions are grouped by topic, and many include links to the documentation for more information.
{: shortdesc}

## Basic usage
{: #basic}

1.  <span style="color:#003F69">What is the {{site.data.keyword.tradeoffanalyticsshort}} service?</span>

    The {{site.data.keyword.tradeoffanalyticsshort}} service helps decision makers to make better choices while taking into account multiple, often conflicting, goals. The service can support users in making complex decisions, like what mortgage to take or which medical treatment to pursue, or common decisions, like which laptop to purchase.

1.  <span style="color:#003F69">What languages does the {{site.data.keyword.tradeoffanalyticsshort}} client library support?</span>

    The widget is localized to support 13 languages; see the description of the `locale` field in [The TradeoffAnalytics constructor](/docs/services/tradeoff-analytics/client.html#constructor) for a list of supported languages. By default, the library uses the language of the user's browser.

1.  <span style="color:#003F69">Where can I ask questions and get answers to problems I have with the {{site.data.keyword.tradeoffanalyticsshort}} service?</span>

    You can submit comments or ask questions about the service in the [{{site.data.keyword.watson}} forum ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/tradeoff-analytics/){: new_window}; use the tag **{{site.data.keyword.tradeoffanalyticsshort}}**. You can also ask questions on [StackOverflow ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://stackoverflow.com/questions/tagged/ibm-watson-cognitive){: new_window}.

## Decision problem
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

## Problem resolution
{: #resolution}

1.  <span style="color:#003F69">How does the service categorize options as best candidates, excluded candidates, or incomplete?</span>

    *Best candidates* have no competing options (also referred to as solutions) better than they are for all objectives; *excluded candidates* are those for which at least one option is clearly superior for all objectives; and *incomplete* options fail to specify values for all objectives. For more information, see [Understanding candidates](/docs/services/tradeoff-analytics/visualization.html#understanding).

1.  <span style="color:#003F69">Can I further refine the set of best candidates to identify those most likely to satisfy my needs?</span>

    Set the `find_preferable_options` query parameter of the `POST /v1/dilemmas` method to `true` to produce a subset of preferred solutions. The service analyzes the best candidates and chooses the subset that it believes will satisfy the most users, and it provides a score that indicates its confidence in its selections. For more information, see [Requesting preferable options](/docs/services/tradeoff-analytics/input.html#preferable).

1.  <span style="color:#003F69">How can I learn why an option was excluded?</span>

    The service lists all excluded candidates by name in the **Excluded** tab under the **Table** view in the second step of the interface process. For more information, see [Excluded candidates](/docs/services/tradeoff-analytics/visualization.html#excluded).

1.  <span style="color:#003F69">How can I learn what data is missing for an incomplete option?</span>

    The service lists all incomplete candidates by name in the **Excluded** tab under the **Table** view in the second step of the interface process. For more information, see [Incomplete candidates](/docs/services/tradeoff-analytics/visualization.html#incomplete).

1.  <span style="color:#003F69">How do I interpret the `shadows` and `shadow_me` fields of a problem resolution?</span>

    The service uses these fields to indicate when its analysis determines that one option is better than (shadows) another option. When such a relationship is found to exist between two options, the fields are included in the `Solution` JSON that is returned for the options as part of the problem resolution. For more information about interpreting these and related fields of a problem resolution, see [Need explanation of Solution response from {{site.data.keyword.tradeoffanalyticsshort}} API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/questions/191441/need-explanation-of-solutionperspective-response-f.html#answer-191459){: new_window} on the {{site.data.keyword.watson}} forum.

## Visualization interface
{: #visualization}

1.  <span style="color:#003F69">How do I interpret the map visualization?</span>

    The map visualization provides a graphical overview of the best candidates for the problem; it omits excluded and incomplete candidates. For more information, see [Working with the map visualization](/docs/services/tradeoff-analytics/visualization.html#map).

1.  <span style="color:#003F69">How do I interpret the line-based, non-map visualization?</span>

    The line-based visualization, officially referred to as *parallel coordinates*, is another type of visual display for the best candidates of the problem resolution. For more information, see [Working with the parallel-coordinates visualization](/docs/services/tradeoff-analytics/visualization.html#pc).

1.  <span style="color:#003F69">Are there recommended guidelines to follow during the decision-making process?</span>

    The recommended approach is implemented via the {{site.data.keyword.tradeoffanalyticsshort}} interface, which features a three-step decision-making process. The process follows Ben Shneiderman's rubric, "Overview first, zoom and filter, then details-on-demand" ([Shneiderman, 1996](/docs/services/tradeoff-analytics/references.html#shneiderman1996).) For complete information about the process, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html).

1.  <span style="color:#003F69">Are there best practices for working with the map visualization?</span>

    To make a more informed decision and to minimize regret, consider the following guidelines for using the visualization:
    1.  Start by observing the map visualization with the entire set of best candidates and look for clusters of similar candidates.
    1.  Try to understand the relations among objectives: Are they correlated or inverted? Where are the special cases (extreme candidates)? Where are the compromises?
    1.  Filter the candidates according to your preferences, and click on particular candidates to examine their details.
    1.  Add good candidates to your list of **Candidates Added for Comparison** and see whether the tool suggests alternatives to consider for better tradeoffs.
    1.  Make detailed comparisons to realize gains and losses between two candidates at a time from your list of added candidates.
    1.  If you have sufficient time, try to remove or add objectives and determine whether favorable candidates were added or removed as a result.

1.  <span style="color:#003F69">Is it possible to format objective values with the {{site.data.keyword.tradeoffanalyticsshort}} client library (for example, to add a euro symbol to a price value)?</span>

    Yes. For a `numeric` objective, you can specify the number of decimal places, a currency symbol, a prefix, or a suffix; for a `datetime` objective, you can specify the format of the date and time. For more information, see [Formatting numbers and dates](/docs/services/tradeoff-analytics/input.html#formats).
