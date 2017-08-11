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

# Release notes
{: #release-notes}

The following sections document the new features and changes that were included with each release and update of the {{site.data.keyword.tradeoffanalyticsshort}} service.
{: shortdesc}

## 20 February 2017
{: #February2017}

The `POST /v1/dilemmas` method now offers an optional `find_preferable_options` query parameter that directs the service to analyze the best candidates to produce a smaller subset of preferred solutions. This subset usually contains no no more than five solutions, typically three or fewer, that are likely to satisfy the greatest number of users. The service also returns a score that represents the percentage of users that it believes will choose one of the preferred solutions. The results are included as part of the `Resolution` object that the service returns for all decision problems. For more information, see [Requesting preferable options](/docs/services/tradeoff-analytics/input.html#preferable) and [The PreferableSolutions object](/docs/services/tradeoff-analytics/output.html#preferable).

## 30 November 2016
{: #November2016}

The `Solution` object now includes an `excluded_by` field that describes which options were strictly better than an `EXCLUDED` option. For each superior option, the information includes a list of the columns (objectives) whose values are better than the option in question and the differences between the two. For more information, see [The ExcludedBy object](/docs/services/tradeoff-analytics/output.html#excludedBy).

## 5 April 2016
{: #April2016}

Version 2 of the {{site.data.keyword.tradeoffanalyticsshort}} client library is now generally available for production use. Version 2 of the widget provides a greatly improved user experience. It is an updated version of the beta widget that was made available in December 2015.

> **Note:** {{site.data.keyword.IBM_notm}} will continue to support version 1 of the widget until April 5, 2017. Although version 1 continues to be available, it will no longer be updated; version 2 will continue to be enhanced with new features over time.

New users are strongly encouraged to use version 2 of the widget with their applications. Similarly, existing users are strongly encouraged to upgrade their applications to the new version of the widget by making the changes described in [Migrating to the version 2 widget](#migrating). (For documentation about the version 1 widget, see [Using the version 1 widget](#v1widget).)

### Enhancements and changes

Version 2 of the widget includes the following enhancements and changes:

-   The widget now offers a guided, three-phase user experience. In the first step, **Select Criteria**, you select and define the problem objectives. In the second step, **Overview + Add for Comparison**, the widget returns the best candidates from among the available options; you can filter the objectives to focus on the goals that matter most to you and select up to six candidates for closer consideration. In the third and final step, **Compare + Finalize**, you conduct detailed comparisons of pairs of candidates and make a final decision.
-   The widget can now use token-based authentication to communicate with the {{site.data.keyword.tradeoffanalyticsshort}} service; the version 1 widget did not support authentication tokens. The use of proxy-based communication is still supported. You specify the type of authentication to be used with fields of the `options` object that you pass to the `TradeoffAnalytics` constructor. For a high-level overview of tokens and their use with {{site.data.keyword.watson}} services, see [Tokens for authentication ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/doc/common/getting-started-tokens.html){: new_window}.
-   The widget now offers metadata for tracking how users are accessing your application. The widget and the service generate some metadata automatically; you can provide additional metadata specific to your application. You pass metadata to the widget via the `metadata` field of the `options` parameter of the widget's constructor and the `metadata` parameter of the widget's `show` method. For more information about passing metadata and the types of metadata available, see [Tracking usage for analysis](/docs/services/tradeoff-analytics/client.html#tracking).
-   With the exception of its `start` method, which allows you to pass an optional callback function to learn when the widget is ready to receive a problem, the widget no longer supports argument-based callback functions for event notification. Event notification is now handled through the widget's event subscription interface, which has not changed. However, the available notification topics have all changed. The simpler set of event topics now includes only `onError`, `problemResolved`, `optionClicked`, `compareClicked`, and `doneClicked`. For more information, see [Using event notifications](/docs/services/tradeoff-analytics/client.html#notifications) and [Event notification topics](/docs/services/tradeoff-analytics/client.html#topics).
-   The widget now supports three additional languages: Arabic (`ar`), Hebrew (`he`), and Portuguese (`pt`); the widget also continues to support the same ten languages that it did before. You specify the language via the `locale` field of the constructor's `options` argument. For more information, see [The {{site.data.keyword.tradeoffanalyticsshort}} constructor](/docs/services/tradeoff-analytics/client.html#constructor).
-   The widget now supports bidirectional locales. The interface direction changes automatically according to the locale. You can use the `bidiTextDirection` field of the `options` parameter of the widget's constructor to specify the text direction (`rtl` for right to left, or `ltr` for left to right), or you can let the widget choose the optional direction when the language of the text is not known in advance (`auto`). For more information, see [The {{site.data.keyword.tradeoffanalyticsshort}} constructor](/docs/services/tradeoff-analytics/client.html#constructor).
-   The widget now lets you pass a Cross-Site Request Forgery (CSRF) token if you use one to protect your token or proxy endpoints from CSRF attacks. You can use the `csrfToken` field of the `options` parameter of the widget's constructor to pass the token. For more information, see [The {{site.data.keyword.tradeoffanalyticsshort}} constructor](/docs/services/tradeoff-analytics/client.html#constructor).
-   The CSS themes that are used to control the widget's look and feel are now based on Sass technology; LESS technology is no longer used. In addition, the themes made available with the widget are now `Bright` (the default) and `Dark`. You now specify a theme via the `themeCssUrl` field of the `options` parameter of the widget's constructor. (The field used to be named `customCssUrl`.) For more information, see [Customizing the widget's appearance](/docs/services/tradeoff-analytics/client.html#usingThemes).
-   The use of profiles to control the activated features of the widget is not currently supported.

The following sections of the documentation describe version 2 of the widget:

-   [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html)
-   [Programming with the widget](/docs/services/tradeoff-analytics/client.html)

### Migrating to the version 2 widget
{: #migrating}

If you are an existing user, make the following changes to your application code to migrate to version 2 of the widget. These changes address the differences between the two versions.

-   [Integrating the widget in an application](/docs/services/tradeoff-analytics/client.html#integrating) describes the steps for using the widget with your code. The steps have changed as follows:
    -   In *step 1*, the URL of the widget specified by the `src` argument of the `<script>` tag is now `2.0` instead of `v1`:

        ```javascript
        <script type="text/javascript" src="http://ta-cdn.mybluemix.net/2.0/TradeoffAnalytics.js"></script>
        ```
        {: codeblock}

    -   In *step 2*, the order of the `node` and `options` arguments has changed. The `node` argument is now required and the `options` argument is now optional.

        ```javascript
        var taClient = new TradeoffAnalytics(node /* String or DOM node */, options /* Object */);
        ```
        {: codeblock}

    -   The ordering of the methods called in *steps 3, 4, and 5* is now `start`, `show`, and `subscribe`. In addition, the second argument to the `show` method is now a `metadata` object, not a `callback` function.
    -   The steps no longer include the `resize` method, which is no longer supported. Version 2 of the widget handles resizing automatically.
    -   The `destroy` method called in the final step no longer accepts an optional `callback` argument.

        ```javascript
        taClient.destroy();
        ```
        {: codeblock}

-   [The TradeoffAnalytics constructor](/docs/services/tradeoff-analytics/client.html#constructor) describes the fields of the object passed via the `options` argument. The fields have changed as follows.
    -   The `useProxy` field is new. The widget now supports the use of authentication tokens in addition to service credentials for establishing secure communications with the service. This boolean field indicates whether the widget is to use a proxy (`true`) or token (`false`).
    -   The value of the `dilemmaServiceUrl` field now depends on the value of the `useProxy` field. When you use tokens, the application contacts the {{site.data.keyword.tradeoffanalyticsshort}} service directly.
    -   The `tokenServiceUrl` field is new. When you use tokens, this field specifies the URL of a server in the same domain as the application that obtains a token for the application to contact the {{site.data.keyword.tradeoffanalyticsshort}} service directly.
    -   The `csrfToken` field is new. If you use a CSRF token to protect your endpoints, use this field to pass the token with the constructor.
    -   The `themeCssUrl` field replaces the former `customCssUrl` field for specifying the theme to be used by the widget. The predefined and default themes have also changed, as has the technology you use to create a custom theme.
    -   The `profile` field has been removed. The use of profiles to control the activated features of the widget is not currently supported.
    -   The `locale` field now supports three additional languages, `ar` (Arabic), `he` (Hebrew), and `pt` (Portuguese), in addition to the previous ten languages.
    -   The `bidiTextDirection` field is new. It controls the direction of the text in the interface.
    -   The `errCallback` field is no longer supported. You now handle event notifications with the dedicated subscription interface, not with argument-based callback functions.
    -   The new `sendMetadata`, `metadata` and `sendLocationMetadata` fields support optional usage tracking for analytics.
-   [Using event notifications](/docs/services/tradeoff-analytics/client.html#notifications) describes the subscription interface, which has not changed. However, the available event notification topics are now `onError`, `problemResolved`, `optionClicked`, `compareClicked`, and `doneClicked` (see [Event notification topics](/docs/services/tradeoff-analytics/client.html#topics)). Argument-based callback functions for event notification (with the exception of the `start` method) are no longer supported.
-   [Tracking usage for analysis](/docs/services/tradeoff-analytics/client.html#tracking) describes the new metadata available for tracking how users are accessing your application. The use of metadata is optional.
-   [Customizing the widget's appearance](/docs/services/tradeoff-analytics/client.html#usingThemes) describes the new technology and steps for creating a custom CSS theme for the widget. In addition, the predefined themes are now `Bright` (the default) and `Dark`; the three themes available with the version 1 widget are no longer supported. You now specify a theme via the `themeCssUrl` field of the `options` parameter of the widget's constructor. (The field was named `customCssUrl` for the version 1 widget.)
-   The use of profiles to control the activated features of the widget is not currently supported.
-   The `format` field of the `Column` object now relies on AngularJS formatting patterns to specify how option values appear in the widget. Formats for objective values are now specified differently:
    -   For `numeric` columns, the field supports `number`, `currency`, `taPrefix`, and `taSuffix` formatters, as well as combinations of the formatters.
    -   For `datetime` columns, the field supports a `date` formatter.

    For information about column formatting with version 2 of the widget, see [Formatting numbers and dates](/docs/services/tradeoff-analytics/input.html#formats).

### Using the version 1 widget
{: #v1widget}

The {{site.data.keyword.tradeoffanalyticsshort}} documentation now describes version 2 of the widget. The following pages provide information about using the version 1 widget. These pages are separate from the current documentation and accessible only from these release notes.

-   [Using the widget (version 1)](/docs/services/tradeoff-analytics/visualization-v1.html)
-   [Programming with the widget (version 1)](/docs/services/tradeoff-analytics/client-v1.html)

If you are using version 1 of the widget, use the following patterns for the `format` field of the `Column` object:

-   For `numeric` columns, specify a number pattern according to the Unicode number format pattern; for example, `"format": "####0.00"` and <code>"format": "&euro;####0.00"</code>. For more information, see [Section 3.1 of the Unicode Technical Standard #35 ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://www.unicode.org/reports/tr35/tr35-numbers.html#Number_Format_Patterns){: new_window}. The field does not support use of the Unicode currency symbol (&#164;).
-   For `datetime` columns, specify a date pattern according to the `datePattern` field of the `dojo.date.locale.format` function; for example, `"format": "MMM d, yyyy"` and `"format": "h:m:s a"`. For more information, see the description of the `datePattern` field in the [Dojo Toolkit Reference Guide ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://dojotoolkit.org/reference-guide/1.6/dojo/date/locale/format.html){: new_window}.

### Miscellaneous documentation updates

The following general notes apply to the {{site.data.keyword.tradeoffanalyticsshort}} documentation:

-   In many cases, object names have been simplified; for example, `ColumnDefinition` is now `Column` and `SolutionPerspective` is now `Solution`. These names are transparent to developers, but the changes simplify the descriptions of the objects in the documentation.
-   [Submitting a decision problem](/docs/services/tradeoff-analytics/input.html) now clearly indicates the fields of the `Column` and `Option` objects that are used only by the widget and do not contribute to the problem definition. For the `Column` object, the fields are `format`, `full_name`, and `description`; for the `Option` object, the fields are `name`, `description_html`, and `app_data`.
-   Descriptions of the `Map` object elide information that is used only internally by the widget, including the `comments` field, the `config` field and its `MapConfig` object, and the `metrics` field and its `MapQualityMeasurements` object. The `Map` object is returned by the `dilemmas` method when the `generate_visualization` query parameter is `true`.
-   The detailed discussion of the previous sample application is no longer provided. Refer to the source code for the sample application in GitHub instead.

## Older releases
{: #older}

-   [7 December 2015](#December2015)
-   [28 May 2015](#May2015)

### 7 December 2015
{: #December2015}

The {{site.data.keyword.tradeoffanalyticsshort}} service was updated on December 7, 2015, to provide a new beta user interface for the client library. The new interface provides a guided experience that makes multiple-criteria decision making much more approachable. For an introduction to the interface, see the blog post [Announcing a new UI widget for the {{site.data.keyword.tradeoffanalyticsshort}} service ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/watson/blog/2015/12/07/announcing-a-new-ui-widget-for-the-tradeoff-analytics-service/){: new_window}.

### 28 May 2015
{: #May2015}

As of May 28, 2015, the {{site.data.keyword.tradeoffanalyticsshort}} service is generally available (GA). To begin using the new GA version of the service with an existing application, you need to delete the instance of the beta service, create a new instance of the GA service, bind the new instance to your application, and restage the application. This is a common operation that requires no changes to the source code for your application.

The following differences exist between the beta and GA versions of the {{site.data.keyword.tradeoffanalyticsshort}} API. The GA version of the API is backward-compatible with the beta version. You do not need to change your existing application code for it to work with the GA version of the service.

The service's `dilemmas` method has changed as follows. For more information about the GA version of the `dilemmas` method, see [Submitting a decision problem](/docs/services/tradeoff-analytics/input.html), [Understanding a problem resolution](/docs/services/tradeoff-analytics/output.html), and the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/tradeoff-analytics/api/v1/){: new_window}.

-   The `type` field of the `Column` object now supports two additional column types: `datetime` for date and time values, and `categorical` for categories of values. The field continues to support the `numeric` and `text` types.
-   The `range` field of the `Column` object now supports the definition of ranges for `datetime` and `categorical` column types. The field's `IRange` argument can be a `ValueRange` for `numeric` columns, a `DateRange` for `datetime` columns, or a `CategoricalRange` for `categorical` columns.
-   The `Column` object supports a new `preference` field for specifying the preferred order for a subset of the values of a `categorical` column type.
-   The `format` field of the `Column` object can be used to format option values for both `numeric` and `datetime` column types.
-   The `enum_vals` field of the `Column` object has been removed. The field was reserved for use by the service.
-   The `status` field of the `Solution` object can now return a `DOES_NOT_MEET_preFERENCE` status for options that specify a value for a `categorical` column that is not among the preferences for that column.
-   The `error_code` field of the `StatusCause` object can now report a `DOES_NOT_MEET_preFERENCE` value for options that provide a value that is not among the preferences for a `categorical` column. In addition, the field can no longer report a `missing_design_value`.
-   Some fields of the interface that were previously mandatory are now optional. This includes the `type` field (the default is now `text`) and the `goal` field (the default is now `max`) of the `Column` object; the `name` field of the `Option` object; and the `config` field of the `Map` object.
-   The use of uppercase strings for constant values of the API is now deprecated. Lowercase strings are now used for constant values.

The {{site.data.keyword.tradeoffanalyticsshort}} client library (widget) has changed as follows:

-   The URL of the widget on the {{site.data.keyword.tradeoffanalyticsshort}} Content Distribution Network (CDN) has changed from `https://ta-cdn.mybluemix.net/TradeoffAnalytics.js` to `http://ta-cdn.mybluemix.net/v1/TradeoffAnalytics.js` (note the addition of the `/v1` component to the path).
-   The `TradeoffAnalytics` object is now named `TradeoffAnalytics`. Calls to the constructor for the widget are now of the form `new TradeoffAnalytics (options, node)`. The previous form of the name is still supported.
-   The signatures of some of the methods of the `TradeoffAnalytics` object from the library have changed to support other modifications described in this list. For more information about the methods and their arguments, see [Integrating the widget in an application](/docs/services/tradeoff-analytics/client.html#integrating).
-   The client library now supports event notifications. The hosting application can subscribe to topics to receive notifications from the client library. The notifications allow the application to respond to changes to the client state as a result of user interactions. You can still specify callback functions as arguments to functions that supported them in the beta version of the service. For more information about using event notifications, see [Using event notifications](/docs/services/tradeoff-analytics/client.html#notifications). For
-   The library has been localized to support additional languages for the visualization interface. The widget now supports the following 10 languages: Brazilian Portuguese (*pt-br*), Chinese (*zh*), Chinese (Taiwan) (*zh-tw*), English (*en*), French (*fr*), German (*de*), Italian (*it*), Japanese (*ja*), Korean (*ko*), and Spanish (*es*). For more information, see [The TradeoffAnalytics constructor](/docs/services/tradeoff-analytics/client.html#constructor).
-   The user interface of the visualization includes a number of changes to improve how you interact and experiment with a problem resolution. Some of the changes reflect improvements to the service's APIs; others enhance the general usability of the interface. For more information about using the interface, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html).

Finally, the scope of the {{site.data.keyword.tradeoffanalyticsshort}} documentation has expanded considerably. It now includes more information about the input and output of the service's API, the widget and its usage, use cases, real-world applications, the research that underlies the service, and a collection of useful FAQs.
