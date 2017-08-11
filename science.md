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

# The science behind the service
{: #science}

Decision makers often need to take into account multiple conflicting objectives when selecting a solution for a problem. They might also need to consider a potentially large number of candidate options. The {{site.data.keyword.tradeoffanalyticsshort}} service helps decision makers reach a sustained decision by providing a means to cope with the process of selecting the candidate that best meets their preferences.
{: shortdesc}

The two pillars of the {{site.data.keyword.tradeoffanalyticsshort}} service are identifying the best candidate options from among many possible candidates and helping the decision maker select one of those options. The service assists the decision maker by providing innovative visual representations, analytics, and guidance that improve the selection process and encourage exploration. This process stimulates and supports careful yet successful human judgment. The following sections describe how the service obtains the best candidates, supports exploration, and provides decision analytics as judgment aids to the decision maker.

## Obtaining the best candidates
{: #obtaining}

Finding the best candidates for a decision problem entails reducing the number of options to yield a smaller set of optimal solutions. Definition of the optimal solutions, referred to as *Pareto Optimality* ([Ehrgott, 2005](/docs/services/tradeoff-analytics/references.html#ehrgott2005)), necessitates the formulation of a partial order among the objectives. This partial order, referred to as *Pareto Domination*, states that one option (an objective vector) dominates another if the following conditions hold:

1.  All of the dominant option's values are at least as good as the other option's values.
1.  At least one of the dominant option's values is strictly better than the corresponding value of the other option.

Pareto Optimality thus aims to obtain the nondominated set of options for the objectives that are to be minimized or maximized for the decision problem. This set of options, called the *Pareto Frontier*, constitutes the potentially optimal selections with respect to the specified objective functions.

In contrast to the Pareto Frontier, traditional approaches consider multi-objective problems by posing a weighted sum of the problems' objectives to obtain an individual solution point. These traditional methods do not provide a complete view of the conflicts that exist among the objectives (that is, they do not reveal the Pareto Frontier). They therefore hinder a thorough and careful decision process, one that ensures a sustained result as described by [Janis and Mann (1977)](/docs/services/tradeoff-analytics/references.html#janis1977). The traditional approaches also necessitate a priori prescription of the objectives' weighting. Other mathematical limitations are described by [Das and Dennis (1997)](/docs/services/tradeoff-analytics/references.html#das1997).

## Exploring the best candidates
{: #exploring}

Visualizing a Pareto Frontier, the optimal set of solutions to a multi-objective problem, is considered a difficult task when the problem spans more than three objective functions. Many techniques use multiple displays to show several objectives at a time.

For example, Interactive Decision Maps ([Lotov et al., 2004](/docs/services/tradeoff-analytics/references.html#lotov2004)) employ scatter-plot matrices to display bi-objective slices of the frontier in which each slice is a scatter-plot of two objectives. The points are colored according to the value of the third objective. All other *m - 3* objectives, where *m* is the total number of objectives, are set to certain values. Using sliders, the user can change the values of the *m - 3* objectives and explore the frontier. [Andrienko et al. (2003)](/docs/services/tradeoff-analytics/references.html#andrienko2003) enhanced this approach by using a utility bar chart as a means to both filter and compare the visible options.

To date, the Parallel Coordinates method ([Inselberg &amp; Dimsdale, 1990](/docs/services/tradeoff-analytics/references.html#inselberg1990)) is the prevalent technique used to visualize Pareto Frontiers ([Bagajewicz &amp; Cabrera, 2003](/docs/services/tradeoff-analytics/references.html#bagajewicz2003)). The major advantage of this technique is its ability to easily compare solutions across all objectives. However, as the number of solutions increases, the display becomes cluttered and less clear, making it difficult to gain insight and to identify patterns across the entire frontier without filtering the solutions. A review by [Korhonen and Wallenius (2008)](/docs/services/tradeoff-analytics/references.html#korhonen2008) provides more information about techniques for visualizing Pareto Frontiers.

The {{site.data.keyword.tradeoffanalyticsshort}} service introduces a novel visual-interactive approach to facilitate coping with multi-objective problems. The service follows Shneiderman's visual information-seeking mantra: "Overview first, zoom and filter, then details-on-demand" ([Shneiderman, 1996](/docs/services/tradeoff-analytics/references.html#shneiderman1996)). User exploration starts with an overview of the map. By using the vertices as reference points, users are able to judiciously explore the map. At any point, users can employ the filter sliders to denote their preferences. The solutions that do not meet their preferences are then grayed out. When the user hovers over a specific solution, a tool tip pops up to provide more details and to enable certain interactions. Finally, users can select the nearest neighboring solutions to examine them in more detail by using either the existing map or additional views.

Effectively visualizing a Pareto Frontier requires attributing equal importance to each of the objectives. This approach requires equal representation of the visualization dimensions to support equal interpretation of the dimensions by the user. In its map visualization, the {{site.data.keyword.tradeoffanalyticsshort}} service represents the *m* objectives by the corners of the map. It accomplishes this representation by using a symmetric *m*-gon, a polygon with *m* edges of equal length, and positioning the objectives at all *m* vertices.

To effectively present the Pareto Frontier as a whole, a visualization must represent individual solutions in a way that enables their values to be interpreted easily. The {{site.data.keyword.tradeoffanalyticsshort}} service augments the plain Self-Organizing Map (SOM) ([Kohonen, 2001](/docs/services/tradeoff-analytics/references.html#kohonen2001)) to explicitly represent the dimensions by using a so-called SOM anchor as a fixed point for each objective. (For an in-depth description of the related neural network learning algorithm, see [Chen et al., 2013](/docs/services/tradeoff-analytics/references.html#chen2013).)

Finally, the service uses large-scale glyphs to provide full-data extraction and easy exploration of the tradeoffs among the objectives. Each objective is assigned a color according to Colorbrewer's qualitative color scale ([Brewer &amp; Harrower, 2002](/docs/services/tradeoff-analytics/references.html#brewer2002)). The service uses radial bar charts (polar area charts) as its glyphs due to their economical use of space. The service does not use profile glyphs such as bar charts because the space they occupy depends on the number of objectives. Similarly, the service does not use pie charts because they implicitly denote the ratio of the objectives rather than their magnitude, which radial bar charts capture well.

## Decision analytics as judgment aids
{: #analytics}

The {{site.data.keyword.tradeoffanalyticsshort}} service offers guidance according to the fundamental research of [Janis and Mann (1977)](/docs/services/tradeoff-analytics/references.html#janis1977), who note that consequences and tradeoffs need to be obvious to the decision maker. Careful assessment of all available alternatives is necessary, for example, to minimize regrets (such as in a commerce scenario), to ensure patient persistence with a treatment protocol despite adverse effects and pain, and to encourage investor confidence and endurance despite market volatility.

Consequently, the service applies analytical techniques to the process of choosing from among Pareto Optimal solutions. These techniques enable machine-driven calculations that might prove too cumbersome for human perception given a high-dimensional data set of large volume. For instance, [Amid et al. (2014)](/docs/services/tradeoff-analytics/references.html#amid2014) describes the countless pairwise comparisons that the service performs to determine the cases where small insignificant loss on one (or more) objectives might be worth significant gains in other objectives. This approach helps the service minimize regret by highlighting options that the decision maker might otherwise miss.

[Shir et al. (2014)](/docs/services/tradeoff-analytics/references.html#shir2014) details further research done by {{site.data.keyword.IBM_notm}} that follows Kahneman and Tversky's prospect theory ([Kahneman &amp; Tversky, 1979](/docs/services/tradeoff-analytics/references.html#kahneman1979)). This work aims to analyze the landscape of options by identifying a recommended subset of the options on the Pareto Frontier that accounts for the decision-maker's tendencies. This approach offers the decision maker a gain-prone or loss-averse subset of options in an interactive fashion.
