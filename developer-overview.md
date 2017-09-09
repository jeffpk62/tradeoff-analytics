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

# Overview for developers
{: #developerOverview}

You can access the capabilities of the {{site.data.keyword.tradeoffanalyticsshort}} service via an HTTP Representational State Transfer (REST) API. Software Development Kits (SDKs) are also available to simplify application development in various languages. The following sections provide an overview of application development with the service.
{: shortdesc}

## Programming with the service
{: #programming}

To use the {{site.data.keyword.tradeoffanalyticsshort}} service, you pass a decision problem to the HTTP `POST dilemmas` method of the service's API. You represent a decision problem as a JSON object. The following example shows how such a construct relates to the tabular representation of a simple problem.

![Tradeoff Analytics API Example](images/TA-Table-Example.png)

The example includes two objectives specified via the `columns` parameter with `key`s **obj1** and **obj2**. Each has a `full_name` that describes the objective, a `type` of **numeric**, and the boolean parameter `is_objective` set to **true** to indicate that the service is to evaluate each option against it. The `goal` of the first objective specifies that it is to be minimized (**min**), while that of the second indicates that it is to be maximized (**max**).

The JSON further specifies two options via the `options` parameter. Each option has a `key` and a `name`, and each includes `values` for both of the objectives. The image shows how the objectives and options specified in the JSON relate to the decision problem.

The service returns a `Dilemma` in the form of a JSON object that contains two constructs: the original `problem` and its `resolution`. The `resolution` includes `solutions` that describe how the options stack up against both the objectives and each other. By default, the `resolution` also includes a `map` with the parameters needed to represent the problem and its solutions on the two-dimensional visualization in the service's graphical interface.

The service makes its client interface available via a JavaScript library that provides a powerful web client with which users can explore and analyze the tradeoffs among the options. The library, referred to as the {{site.data.keyword.tradeoffanalyticsshort}} widget, is the recommended means of consuming the service's output. It is available from the {{site.data.keyword.tradeoffanalyticsshort}} Content Distribution Network (CDN).

Use of the interactive interface is optional, and an application can call the service without invoking the interface. The service can return just the results of its analysis without including the parameters needed for the interface and its visualization. For more information about working with the interactive interface, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html) and [Programming with the widget](/docs/services/tradeoff-analytics/client.html).

## Using Software Development Kits
{: #sdks}

The {{site.data.keyword.tradeoffanalyticsshort}} service supports a number of SDKs for simplified application development. The SDKs are available for many popular programming languages and platforms, including Node.js, Java, Python, and Apple&reg; iOS. For a complete list of SDKs and information about using them, see [{{site.data.keyword.watson}} SDKs](/docs/services/watson/getting-started-sdks.html). All SDKs are available from the [watson-developer-cloud namespace ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud){: new_window} on GitHub.

The service also makes available the following sample applications to help you get started:

-   You can access an application that uses the Node.js SDK at the [tradeoff-analytics-nodejs repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/tradeoff-analytics-nodejs){: new_window}.
-   You can access an application that uses the Java SDK at the [tradeoff-analytics-java repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/tradeoff-analytics-java){: new_window}.

All SDKs support authenticating by using either your {{site.data.keyword.Bluemix_short}} service credentials or an authentication token.

## Learning more about application development
{: #learn}

The {{site.data.keyword.tradeoffanalyticsshort}} service supports two typical programming models: *Direct interaction*, in which the client communicates with the service directly; and *relaying via a proxy*, in which the client and service exchange all data (requests and results) through a proxy application that resides in {{site.data.keyword.Bluemix_short}}.

For more information about working with {{site.data.keyword.watson}} Developer Cloud services and {{site.data.keyword.Bluemix_notm}}, see the following:

-   For an introduction to working with {{site.data.keyword.watson}} services and {{site.data.keyword.Bluemix_notm}}, see [Getting started with {{site.data.keyword.watson}} and {{site.data.keyword.Bluemix_notm}}](/docs/services/watson/index.html).
-   For a language-independent introduction to developing {{site.data.keyword.watson}} services applications in {{site.data.keyword.Bluemix_notm}}, see [{{site.data.keyword.Bluemix_notm}} development approaches](/docs/services/watson/getting-started-bluemix.html).
-   For information about the two programming models available for developing {{site.data.keyword.watson}} applications, see [Programming models for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-develop.html):
    -   With relaying via a proxy, the client relies on a proxy server that resides in {{site.data.keyword.Bluemix_notm}} to communicate with the service; it passes all requests through the proxy application. Relaying requests via a proxy relies only on service credentials to authenticate with the service; see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html).
    -   With direct interaction, the client uses the proxy application in {{site.data.keyword.Bluemix_notm}} only to obtain an authentication token for the service, after which it communicates directly with the service. Direct interaction uses service credentials only to obtain a token; see [Tokens for authentication](/docs/services/watson/getting-started-tokens.html).
-   For information about controlling the default request logging that is performed for all {{site.data.keyword.watson}} services, see [Controlling request logging for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-logging.html).
