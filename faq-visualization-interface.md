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

# Visualization interface
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
