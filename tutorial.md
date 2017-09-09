---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-09"

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

# Tutorial
{: #gettingStarted}

The {{site.data.keyword.tradeoffanalyticsfull}} service helps people make better choices when faced with multiple goals and alternatives. This cURL-based tutorial can help you get started quickly with the {{site.data.keyword.tradeoffanalyticsshort}} service. The examples show you how to call the service's `POST dilemmas` method both with and without requesting the map visualization that is used by the service's client widget, and with and without requesting preferable options. For more information, see [Submitting a decision problem](/docs/services/tradeoff-analytics/input.html) and [Understanding a problem resolution](/docs/services/tradeoff-analytics/output.html).
{: shortdesc}

## Prerequisites
{: #prereqs}

Before starting the tutorial, make sure that you meet the following prerequisites:

-   **Obtain {{site.data.keyword.Bluemix_short}} credentials:** For information about obtaining service credentials for working with a {{site.data.keyword.watson}} service in {{site.data.keyword.Bluemix_notm}}, see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html).
-   **Install the cURL executable:** cURL is a popular command-line tool for calling methods of HTTP interfaces. If you have not already downloaded the `curl` executable, you can install the version for your operating system from [curl.haxx.se ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://curl.haxx.se){: new_window}. You must install the version that supports the Secure Sockets Layer (SSL) protocol. Make sure to include the installed binary file on your `PATH` environment variable.

The examples use the cURL `--user` option to pass valid credentials for the service. In the examples, replace `{username}:{password}` with the values of the *username* and *password* from your service credentials. Note that you must use your service credentials, *not* your {{site.data.keyword.Bluemix_notm}} ID and password.

## Decision problem input
{: #input}

The examples use the cURL `--data` option to pass the contents of a JSON file, <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/tradeoff-analytics/problem.json" download="problem.json">problem.json <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>, to the service. The file sends a JSON `Problem` object that defines the decision problem. To learn more about the contents of the file, see

-   [Example objectives](/docs/services/tradeoff-analytics/input.html#exampleObjectives)
-   [Example options](/docs/services/tradeoff-analytics/input.html#exampleOptions)

For detailed information about passing input to the service, see [Submitting a decision problem](/docs/services/tradeoff-analytics/input.html).

## Problem resolution without map visualization
{: #withoutMap}

The first example passes the `problem.json` file to the `POST dilemmas` method with the `generate_visualization` query parameter set to `false`. This tells the service to return a `Resolution` object that does not include the `Map` object that is used by the {{site.data.keyword.tradeoffanalyticsshort}} client widget. The example sets the `Content-Type` header to `application/json`, which is required for all calls to the method.

```bash
curl -X POST --user {username}:{password}
--header "Content-Type: application/json"
--data @problem.json
"https://gateway.watsonplatform.net/tradeoff-analytics/api/v1/dilemmas?generate_visualization=false"
```
{: pre}

The service returns a JSON `Dilemma` object that includes a `problem` field. This field duplicates the input `Problem` object exactly as it was submitted. The `Dilemma` object also includes a `Resolution` object that provides the results of the service's evaluation of the seven options of the input file. The `solutions` field of the `Resolution` object provides an array of `Solution` objects with results for each option. For an example of the results for this input file and a description of the information that is returned, see [Example solutions](/docs/services/tradeoff-analytics/output.html#exampleSolutions).

## Problem resolution with preferable options
{: #withPreferable}

The second example is very similar to the first. It passes the same `problem.json` file to the `POST dilemmas` method and again sets the `generate_visualization` query parameter to `false`. But this example also sets the `find_preferable_options` query parameter to `true` to request a refined list of preferable solutions.

```bash
curl -X POST --user {username}:{password}
--header "Content-Type: application/json"
--data @problem.json
"https://gateway.watsonplatform.net/tradeoff-analytics/api/v1/dilemmas?generate_visualization=false&find_preferable_options=true"
```
{: pre}

The service again returns a JSON `Dilemma` object that includes the original input `Problem` object and a `Resolution` object that provides the service's solutions for the problem. These results are identical to those returned for the previous example.

However, the `Resolution` object also includes the `PreferableSolutions` object. This object refines the set of best candidates identified by the service. In this example, the set is reduced from three candidates to two, the remaining options being those most likely to be selected by the largest number of users. For an example with a greater number of options, the set of preferable solutions might reduce the collection of best candidates from a much larger number, making it easier for the user to identify the best candidates. The results include a confidence score that indicates the percentage of users likely to be satisfied with one of the preferred solutions. For an example of the results for this input file and a discussion of their meaning, see [Example preferable solutions](/docs/services/tradeoff-analytics/output.html#examplePreferable).

## Problem resolution with map visualization
{: #withMap}

The third example is again similar to the first two, but it omits the `generate_visualization` parameter to use the default value of `true`. This directs the service to generate the `Map` object as part of its results. It also omits the `find_preferable_options` parameter to use the default value of `false`.

```bash
curl -X POST --user {username}:{password}
--header "Content-Type: application/json"
--data @problem.json
"https://gateway.watsonplatform.net/tradeoff-analytics/api/v1/dilemmas"
```
{: pre}

Because the call submits the same input file, the service returns the same JSON `Dilemma` object, though without the `PreferableSolutions` object. However, the `Resolution` object in this case also includes the `Map` object, which tells the {{site.data.keyword.tradeoffanalyticsshort}} client widget how to display the results of the decision problem. For an example of the `Map` object that is returned for this input file, see [Example map visualization](/docs/services/tradeoff-analytics/output.html#exampleMap). For more information about using the client widget, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html) and [Programming with the widget](/docs/services/tradeoff-analytics/client.html).
