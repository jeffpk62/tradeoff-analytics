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

# Problem resolution
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
