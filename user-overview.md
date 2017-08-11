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

# Overview for users
{: #userOverview}

*"Waiting hurts. Forgetting hurts. But not knowing which decision to take can sometimes be the most painful..."* - Jose N. Harris

*"The hardest thing about the road not taken is that you never know where it might have led."* - Lisa Wingate

*"It does not take much strength to do things, but it requires a great deal of strength to decide what to do."* - Elbert Hubbard

We constantly face tough choices. Which car should I buy? Which college should I attend? Which pharmaceutical drug should be subsidized? The examples are vast and cover all areas of our lives. Making difficult decisions can be time-consuming, and naturally, we want to be sure that our final decision is the best one. Yet even when we do our homework well, we sometimes resort to guessing and then wonder whether we missed a better option.
{: shortdesc}

The only way to be certain of a decision is to conduct thorough research of all available alternatives. We need to define our objectives, the criteria that we care about, and see how the alternatives measure up against them. But this is easier said than done; not only do we need to consider among many options, we also often have to cope with conflicting goals.

The {{site.data.keyword.tradeoffanalyticsshort}} service is designed to help guide you through the process of making difficult choices. The service reduces the pool of options to just the best candidates. It analyzes the tradeoffs among the options and can present the results in an interactive tool with helpful visualizations. The tool helps you to better understand the alternatives.

## How the service works
{: #how}

When making a difficult choice, we are trying to resolve a *decision problem*. We base our decision on a set of meaningful *objectives*, or criteria, which specify the parameters or goals of our decision, and a number of available *options*. We evaluate each option against each objective.

The {{site.data.keyword.tradeoffanalyticsshort}} service is intended to support users in complex decision-making. To illustrate how the service does this, consider the dilemma of buying a new car. We each have personal preferences regarding such an investment. To leverage the {{site.data.keyword.tradeoffanalyticsshort}} service for help with this purchase, a user can employ the guided decision-making interface to do the following:

1.  *Specify the objectives for the purchase.* For instance, the user might specify that the selection criteria include price, power, make, model, and ecofriendliness, and that the selection be made from among a dozen models. The user can customize the objectives, such as minimal price, and prioritize the order of the models.
1.  *Explore the options recommended by the service by using its novel graph and table visualizations.* The service automatically identifies the optimal candidate options based on the objectives. It then presents those candidates to the user in an intuitive interactive interface.
1.  *Use filters to narrow down the options.* To increase or decrease the pool of viable candidates, the user can specify thresholds for objective values. For instance, the user might emphasize price over make and model. The interface makes it easy for the user to reflect these preferences in its visual representation, enabling the user to select a car that excels on pricing but compromises on make and model.
1.  *Add a few candidates for further comparison.* After narrowing down the pool of candidates, the user can add a few compelling options and take them to the comparison phase to examine them in detail.
1.  *Examine the service's insights regarding good tradeoffs that might have been missed.* In these cases, the service can advise you to replace a selection if a small compromise for that choice can yield larger gains for a replacement choice.
1.  *Compare all candidates in detail and reach a final decision.* By using the parallel coordinates visualization, the user can compare options in detail, understand their tradeoffs, and make an informed decision.

The {{site.data.keyword.tradeoffanalyticsshort}} guided decision-making interface is the recommended means of working with the service. For more information about using the interactive interface, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html).
