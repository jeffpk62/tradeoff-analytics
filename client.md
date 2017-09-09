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

# Programming with the v2 widget
{: #widget}

The {{site.data.keyword.tradeoffanalyticsshort}} widget provides a powerful web client that helps users make educated decisions by comparing their options and exploring their tradeoffs. The widget accepts a JSON `Problem` object that contains the list of options and additional metadata. Its interactive interface then presents an initial panel to get additional information (selected criteria and preferences) from the user.
{: shortdesc}

When the user clicks **HELP ME DECIDE**, the widget calls the service's `dilemmas` method, which returns a JSON `Dilemma` object that contains analytics about the options. The interface presents these results to the user for further exploration. For more information, see [Using the visualization interface](/docs/services/tradeoff-analytics/visualization.html).

You embed the widget in an application by accessing the [TradeoffAnalytics.js ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://ta-cdn.mybluemix.net/2.0/TradeoffAnalytics.js){: new_window} script from the {{site.data.keyword.tradeoffanalyticsshort}} Content Distribution Network (CDN). The script loads the widget inside an iframe and provides an interface for controlling it and for getting notifications from it.

The following sections describe how to integrate and work with the widget in an application. For Node.js sample code to help you get started quickly, see the [{{site.data.keyword.tradeoffanalyticsshort}} Node Starter Application ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/boazdavid/tradeoff-analytics-v2-vanilla-nodejs){: new_window}.

## Integrating the widget in an application
{: #integrating}

To integrate the {{site.data.keyword.tradeoffanalyticsshort}} widget in an application, use the following steps.

> **Note:** The widget calls the {{site.data.keyword.tradeoffanalyticsshort}} service both to display the initial analysis of a decision problem and when the user modifies the list of active objectives for the problem. In both cases, the application is charged for the call to the service.

1.  *Import the client library in your application.* The following code includes the library in an HTML file from its public URL.

    ```javascript
    <script type="text/javascript" src="http://ta-cdn.mybluemix.net/2.0/TradeoffAnalytics.js"></script>
    ```
    {: codeblock}

1. *Create an instance of the widget.* The following code calls the constructor for the `TradeoffAnalytics` object to create a local instance named `taClient`.

    ```javascript
    var taClient = new TradeoffAnalytics(node /* String or DOM node */, options /* Object */);
    ```
    {: codeblock}

    The constructor takes two arguments: the required `node` argument identifies a DOM node from the hosting document in which to place the widget, and the optional `options` argument specifies the parameters with which to create the instance. For more information, see [The TradeoffAnalytics constructor](#constructor).

1.  *Start the widget.* The following code creates the iframe and loads the widget inside of it. The widget invokes the optional callback function when it finishes starting and is ready to receive a problem.

    ```javascript
    taClient.start(callback /* Function */);
    ```
    {: codeblock}

1.  *Show the problem.* The following code obtains an initial resolution for the problem from the service. You can call the `show` method multiple times with different data sets.

    ```javascript
    taClient.show(problem /* Object */, metadata /* Object */);
    ```
    {: codeblock}

    The function takes two arguments. The first is the required `Problem` object; for more information, see [Submitting a decision problem](/docs/services/tradeoff-analytics/input.html). The second is optional metadata that allows you to specify information about the problem for analytical purposes; see [Tracking usage for analysis](#tracking).

1.  *Subscribe to event notifications.* The following code subscribes to a specified topic with the indicated callback function.

    ```javascript
    taClient.subscribe(topic /* String */, callback /* Function */);
    ```
    {: codeblock}

    Notifications allow the hosting application to listen for changes to the client state and respond appropriately; see [Using event notifications](#notifications).

1.  *Remove subscriptions to event notifications.* When you no longer need them, you can remove subscriptions to event notifications for one or all topics by calling the `unsubscribe` method on the instance of a subscription or by calling the `clearSubscriptions` or `clearAllSubscriptions` methods on the instance of the widget; see [Using event notifications](#notifications).
1. *Destroy the instance of the widget.* The following code removes the widget's iframe from the containing DOM node and clears all subscriptions.

    ```javascript
    taClient.destroy();
    ```
    {: codeblock}

### The TradeoffAnalytics constructor
{: #constructor}

You call the `TradeoffAnalytics` constructor to create an instance of the widget for use in your application. The constructor has the following interface:

```javascript
TradeoffAnalytics(node /* String or DOM node */, options /* Object */)
```
{: codeblock}

The required `node` argument specifies a Document Object Model (DOM) node from the hosting document under which to place the widget's iframe. You can specify a node ID (string) or a DOM element object.

The optional `options` argument is a JSON object that specifies the parameters with which the instance is to be created. The following table describes the object's fields, all of which are optional.

<table>
  <caption>Table 1. Fields of the <code>options</code> argument</caption>
  <tr>
    <th style="text-align:left; width:24%">Field</th>
    <th style="text-align:center; width:12%">Type</th>
    <th style="text-align:center; width:24%">Default</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td>
      <code>useProxy</code>
    </td>
    <td style="text-align:center">
      Boolean
    </td>
    <td style="text-align:center">
      <code>false</code>
    </td>
    <td>
      Indicates whether to access the Tradeoff Analytics service
      through a proxy or directly. If <code>false</code>, uses direct
      communication via token-based authentication; if <code>true</code>,
      uses proxy-based communication via service credentials.
    </td>
  </tr>
  <tr>
    <td>
      <code>dilemmaServiceUrl</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      Depends on <code>useProxy</code>
    </td>
    <td>
      The URL for the Tradeoff Analytics service, which the widget
      calls to find the optimal candidate options and to obtain the
      layout of the map visualization.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          If <code>useProxy</code> is <code>false</code>, use the address
          of the service that is accessible via token-based authentication.
          The default is
          <code>https&colon;//gateway. watsonplatform.net /tradeoff-analytics /api/v1/dilemmas</code>.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          If <code>useProxy</code> is <code>true</code>, use the address
          of a proxy server that resides in the same domain as the
          application and delegates calls to the actual service. The default
          is <code>/tradeoff-analytics-proxy/dilemmas</code>.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <code>tokenServiceUrl</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      Depends on <code>useProxy</code>
    </td>
    <td>
      The URL for a server in the same domain as the application that
      obtains an access token through the Watson <code>token</code> API
      and returns the token to the client. The client uses the token to
      communicate directly with the service identified by
      <code>dilemmaServiceUrl</code>.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          If <code>useProxy</code> is <code>false</code>,
          <code>/tradeoff-analytics-token</code>. (For more information
          about using tokens, see
          <a href="/docs/services/watson/getting-started-tokens.html">Tokens
            for authentication</a>.)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          If <code>useProxy</code> is <code>true</code>, the field is
          ignored.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <code>username</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      No default
    </td>
    <td>
      The username for accessing the proxy or token service for your
      application. Specify a username only if you have protected the
      endpoint for your service with HTTP basic authentication credentials.
    </td>
  </tr>
  <tr>
    <td>
      <code>password</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      No default
    </td>
    <td>
      The password for accessing the proxy or token service for your
      application. Specify a password only if you have protected the
      endpoint for your service with HTTP basic authentication credentials.
    </td>
  </tr>
  <tr>
    <td>
      <code>csrfToken</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      No default
    </td>
    <td>
      A Cross-Site Request Forgery (CSRF) token if you use one to
      protect your token or proxy endpoints from CSRF attacks. The
      widget adds the token as an <code>x-csrf-token</code> header
      that the server can check when it handles the endpoint. For
      more information, see
      <a target="_blank" href="https://en.wikipedia.org/wiki/Cross-site_request_forgery">en.wikipedia.org/wiki/Cross-site_request_forgery ![External link icon](../../icons/launch-glyph.svg "External link icon")</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code>themeCssUrl</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      <code>http&colon;//ta-cdn. mybluemix.net /2.0/css/bright.css</code>
    </td>
    <td>
      An absolute URL to a CSS theme for the widget. You can refer to
      one of the predefined stylesheets or create an application-specific
      theme and make it available from the web. For more information, see
      <a href="#usingThemes">Customizing the widget's appearance</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code>locale</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      The browser's locale or <code>en</code>
    </td>
    <td>
      Specifies a language to use with the widget's interface. By default,
      the widget uses the locale specified by the browser in which the
      hosting application is running. If the widget does not support the
      browser's locale, it defaults to using English. Use this argument
      only if you need to enforce a specific locale.
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>ar</code> (Arabic)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>de</code> (German)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>en</code> (English)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>es</code> (Spanish)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>fr</code> (French)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>he</code> (Hebrew)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>it</code> (Italian)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>ja</code> (Japanese)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>ko</code> (Korean)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>pt</code> (Portuguese)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>pt-br</code> (Brazilian Portuguese)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>zh</code> (Chinese)
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>zh-tw</code> (Taiwanese Chinese)
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <code>bidiTextDirection</code>
    </td>
    <td style="text-align:center">
      String
    </td>
    <td style="text-align:center">
      <code>auto</code>
    </td>
    <td>
      The interface direction changes automatically according to the
      locale. Use this field to specify the text direction to be used:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>ltr</code>: Left-to-Right, the natural text direction for
          languages such as English, German, and French.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>rtl</code>: Right-to-Left, the natural text direction for
          languages such as Arabic and Hebrew.
        </li>
        <li style="margin:10px 0px; color:#404040; line-height:120%;">
          <code>auto</code>: Contextual text direction, which is the optimal
          choice when the language of the text is not known in advance.
        </li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>
      <code>sendMetadata</code>
    </td>
    <td style="text-align:center">
      Boolean
    </td>
    <td style="text-align:center">
      <code>true</code>
    </td>
    <td>
      Indicates whether to send metadata with each service call; see
      <a href="#tracking">Tracking usage for analysis</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code>metadata</code>
    </td>
    <td style="text-align:center">
      Object
    </td>
    <td style="text-align:center">
      No default
    </td>
    <td>
      Metadata that provides information about the instance of the
      <code>TradeoffAnalytics</code> widget; see
      <a href="#tracking">Tracking usage for analysis</a>.
    </td>
  </tr>
  <tr>
    <td>
      <code>sendLocationInMetadata</code>
    </td>
    <td style="text-align:center">
      Boolean
    </td>
    <td style="text-align:center">
      <code>false</code>
    </td>
    <td>
      Indicates whether to send location metadata with each service
      call. If <code>true</code>, browsers request permission from users
      to send their location. See <a href="#tracking">Tracking usage for
        analysis</a>
    </td>
  </tr>
</table>

## Using event notifications
{: #notifications}

The {{site.data.keyword.tradeoffanalyticsshort}} widget allows the hosting application to be notified when certain events occur, for example, when the user makes a final decision at the end of the decision process. To subscribe to a topic, you call the `subscribe` method of the `TradeoffAnalytics` client object. You supply the name of the topic and a callback function; the method returns an object that you can use to unsubscribe from the topic. For a complete list of event notification topics, see [Event notification topics](#topics).

You can use one of three methods to unsubscribe from topics:

-   The `clearSubscriptions` method of the `TradeoffAnalytics` client object takes the name of a topic as its sole argument and removes all subscriptions for the specified topic.
-   The `clearAllSubscriptions` method of the `TradeoffAnalytics` client object takes no argument and clears all subscriptions for all topics.
-   The `unsubscribe` method of a topic object takes no argument and clears the subscription for that specific topic only.

The following example code shows the methods that you can use to subscribe to and to unsubscribe from topics. In the examples, `taClient` is an instance of the `TradeoffAnalytics` widget.

```javascript
function errorHandler(payload){
  // Do something to handle an error. For example, write the error to the console.
  console.error(payload.errorMessage);
}

// Subscribe to the onError topic with the callback function named errorHandler.
var topic = taClient.subscribe('onError', errorHandler);

// Unsubscribe from the specific onError topic.
topic.unsubscribe();

// Remove all subscriptions for the onError topic.
taClient.clearSubscriptions('onError');

// Clear all subscriptions for all topics.
taClient.clearAllSubscriptions();

```
{: codeblock}

### Event notification topics
{: #topics}

The following table describes the topics available with the {{site.data.keyword.tradeoffanalyticsshort}} widget.

<table>
  <caption>Table 2. Available topics</caption>
  <tr>
    <th style="text-align:left">Topic</th>
    <th style="text-align:left">Fires when...</th>
    <th style="text-align:left">Callback parameter type</th>
  </tr>
  <tr>
    <td><code>onError</code></td>
    <td>An error occurs due either to invalid input or
      to a programming error.</td>
    <td>A string that represents the error message.</td>
  </tr>
  <tr>
    <td><code>problemResolved</code></td>
    <td>The service call returns a problem resolution.</td>
    <td>A JSON <code>Dilemma</code> object that contains
      the problem and its resolution.</td>
  </tr>
  <tr>
    <td><code>optionClicked</code></td>
    <td>The user clicks an option.</td>
    <td>A JSON <code>Option</code> object that contains the
      selected option.</td>
  </tr>
  <tr>
    <td><code>compareClicked</code></td>
    <td>The user clicks the <strong>Compare</strong> button in the
      final phase of the decision process.</td>
    <td>A list of the JSON <code>Option</code> objects
      that were selected for detailed comparison.</td>
  </tr>
  <tr>
    <td><code>doneClicked</code></td>
    <td>The user clicks the <strong>Done</strong> button at the end
      of the decision process.</td>
    <td>A JSON <code>Option</code> object that contains the
      option that was selected for the final decision.</td>
  </tr>
</table>

## Tracking usage for analysis
{: #tracking}

Knowing how your users are using the {{site.data.keyword.tradeoffanalyticsshort}} service in your application can provide valuable insights to your business. Such data includes the objectives that users are selecting, the preferences that they are setting for categorical objectives, and so on. With the power of analytics, you can convert this information to novel market insights by answering questions such as

-   What do people look for in a product (common objectives and common preferences for categorical objectives)?
-   Are certain options among the best candidates for all sets of objectives?
-   If your application supports multiple data sets, what data set is used most often?
-   Which users accessed the widget most frequently?

To provide such analytics, the {{site.data.keyword.tradeoffanalyticsshort}} widget sends metadata in addition to the input problem to the service. The widget and the service generate some metadata automatically; see [Metadata generated automatically](#automatic). As an application developer, you can provide other metadata to the widget; see [Metadata provided by the application](#provided). You pass metadata to the widget as follows:

-   Via the `options` parameter of the widget's constructor. Metadata passed via the constructor provides basic information that is appended to the metadata that is automatically generated by the widget and the service.
-   Via the `metadata` parameter of the widget's `show` method. Metadata passed via the `show` method can override or, more commonly, extend the initial data passed to the constructor. In practice, the `dataset-` fields are most relevant for passing via the `show` method, though other data can be useful, as well.

In a request to the service, the metadata fields are concatenated in the format `key=value` and sent via the HTTP header `x-watson-metadata`. A simple example of metadata follows:

```javascript
{
  'app-version': '1.0.0',
  'dataset-id': 'a123'
}
```
{: codeblock}

By default, the widget sends metadata to the service. You can disable metadata by setting the `sendMetadata` field to `false` in the `options` parameter of the widget's constructor.

### Metadata generated automatically
{: #automatic}

The following table describes the built-in metadata that is automatically generated by the widget or the service.

<table>
  <caption>Table 3. Built-in metadata</caption>
  <tr>
    <th style="text-align:left; width:30%">Field</th>
    <th style="text-align:center; width:12%">Type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>service-call-uid</code></td>
    <td style="text-align:center">String</td>
    <td>The universally unique identifier (UUID) of the
      call.</td>
  </tr>
  <tr>
    <td><code>user-agent-uid</code></td>
    <td style="text-align:center">String</td>
    <td>A UUID for the user constructed on a best-effort
      basis by creating and storing a UUID in the widget on the
      client.</td>
  </tr>
  <tr>
    <td><code>client</code></td>
    <td style="text-align:center">String</td>
    <td>The client widget that is used (the current client is
      called <code>TA_WEB_CLIENT</code>).</td>
  </tr>
  <tr>
    <td><code>client-version</code></td>
    <td style="text-align:center">String</td>
    <td>The version of the client widget.</td>
  </tr>
  <tr>
    <td><code>client-locale</code></td>
    <td style="text-align:center">String</td>
    <td>The locale used by the client widget.</td>
  </tr>
  <tr>
    <td><code>client-user-agent</code></td>
    <td style="text-align:center">String</td>
    <td>The system on which the client runs. In the
      widget, this is retrieved from <code>navigator.userAgent</code>.
      In other systems, it may be the type and version of the operating
      system.</td>
  </tr>
  <tr>
    <td><code>client-ip</code></td>
    <td style="text-align:center">String</td>
    <td>The client's IP address, which can be used to learn
      a coarse geographic location. Since the browser does not know its
      host IP, this is added on the server-side by the proxy and thus
      should be implemented by the developer if needed. See the demo
      applications for examples of its use.</td>
  </tr>
  <tr>
    <td><code>client-location</code></td>
    <td style="text-align:center">String</td>
    <td>The client's geographic location (latitude, longitude).
      This is sent only if the developer enables location acquisition by
      setting the field <code>sendLocationInMetadata</code> to
      <code>true</code>. Note that browsers request permission from
      users to send their location.</td>
  </tr>
</table>

### Metadata provided by the application
{: #provided}

The following table describes metadata that can be provided by the calling application. If your application uses a proxy server to communicate with the service, it can add an `ip` field; see the demo applications for examples. Your application can also add its own fields to the metadata, but do not include confidential information in any field of the metadata.

<table>
  <caption>Table 4. Application-defined metadata</caption>
  <tr>
    <th style="text-align:left; width:30%">Field</th>
    <th style="text-align:center; width:12%">Type</th>
    <th style="text-align:left">Description</th>
  </tr>
  <tr>
    <td><code>app-call-context</code></td>
    <td style="text-align:center">String</td>
    <td>A unique identifier for the context in which the
      call to the service was made (for example, from a particular
      page in the application). Specify a human-readable identifier
      that is unique in the scope of the Bluemix organization, space,
      and application.</td>
  </tr>
  <tr>
    <td><code>app-version</code></td>
    <td style="text-align:center">String</td>
    <td>A (sortable) string that identifies the version of
      the calling application or its context.</td>
  </tr>
  <tr>
    <td><code>dataset-name</code></td>
    <td style="text-align:center">String</td>
    <td>A human-readable string that uniquely describes the
      data set.</td>
  </tr>
  <tr>
    <td><code>dataset-id</code></td>
    <td style="text-align:center">String</td>
    <td>A unique identifier for the data set within the
      organization.</td>
  </tr>
  <tr>
    <td><code>dataset-update-time</code></td>
    <td style="text-align:center">Number</td>
    <td>A UNIX timestamp that indicates the last update time
      of the data set. The timestamp is a count of seconds since the
      UNIX Epoch of January 1, 1970 Coordinated Universal Time (UTC).</td>
  </tr>
  <tr>
    <td><code>app-session-id</code></td>
    <td style="text-align:center">String</td>
    <td>A unique identifier that specifies
      application-specific session semantics. If the application needs
      to correlate different instantiations of the widget to a single
      session, it can pass a session identifier in this field.</td>
  </tr>
  <tr>
    <td><code>app-user-id</code></td>
    <td style="text-align:center">String</td>
    <td>A unique, application-specific identifier for the
      user. Do not include a personal identifier such as the user's
      real name or username.</td>
  </tr>
  <tr>
    <td><code>app-user-content</code></td>
    <td style="text-align:center">String</td>
    <td>If the user is acting on behalf of another person
      (for example, in the case of a consultant or a doctor), a unique,
      application-specific identifier for the person being proxied (for
      example, the customer or patient).</td>
  </tr>
</table>

## Customizing the widget's appearance
{: #usingThemes}

You use *themes* to control the look and feel of the widget. You specify the theme, or style, for the widget as CSS. Two predefined themes are available for the widget from the {{site.data.keyword.tradeoffanalyticsshort}} CDN:

-   The *Dark* theme ([dark.css ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://ta-cdn.mybluemix.net/2.0/css/dark.css){: new_window})
-   The *Bright* theme ([bright.css ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://ta-cdn.mybluemix.net/2.0/css/bright.css){: new_window}), the default

You can generate a custom CSS theme file and pass the file to the {{site.data.keyword.tradeoffanalyticsshort}} widget. You generate a custom stylesheet by using Sass technology ([sass-lang.com ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://sass-lang.com/){: new_window}), which supports the use of variables to specify the style. The widget uses a predefined set of variables and default values; for more information about the variables used by the interface, see the file [CSS-Theme-Specification.pdf ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://ta-cdn.mybluemix.net/2.0/themes/TradeoffAnalyticsThemeSpec.pdf){: new_window}. Note that customization of the visualization is not currently supported; for example, you cannot customize the glyph colors and fonts used in the map visualization.

To generate a custom CSS theme for your application, you must have Node.js and `npm` installed on your local machine. Once you do, perform the following steps:

1.  Download the zip file that contains the Sass source files for the {{site.data.keyword.tradeoffanalyticsshort}} service to your local machine from the service's CDN at [http://ta-cdn.mybluemix.net/2.0/themes/themes.zip ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://ta-cdn.mybluemix.net/2.0/themes/themes.zip){: new_window}. Extract the contents of the file.
1.  Change to the directory into which you have extracted the contents of the zip file, and run `npm install` to install the prerequisite software (for example, `gulp`, the Sass compiler, and so on).
1.  Choose one of the two predefined themes as a starting point. Each theme has a corresponding SCSS file. Copy that file and rename it to something unique, such as `myStyle.scss`.
1.  Open the file named `gulpfile.js` in a text editor, and change the `themeFileName` on line 6 to the name of your custom theme file.
1.  Open your custom theme file in a text editor. The file contains variables for the different colors of the widget. Each variable has a comment that specifies the number that appears in the theme specification document. Edit the colors to suit your design preferences.
1.  To create the CSS file, run `gulp compile-theme`.
1.  For your convenience, the `gulp` compiler can regenerate the CSS file each time you save your theme file. To activate this feature, run `gulp` with no arguments from the command line.
1.  Place the generated CSS file in your web project, and pass its full URL to the TradeoffAnalytics constructor by using the `themeCssUrl` field of the `options` parameter; for example:

    ```javascript
    taClient = new TradeoffAnalytics('taWidgetContainer',
      { themeCssUrl: 'http://example.com/custom-tradeoff-analytics-theme.css' });
    ```
    {: codeblock}

If you do not pass a custom theme or pass an invalid theme, the widget applies the *Bright* theme by default.
