# Service Bus HTTP Client
## Requires
- Visual Studio 2010
## License
- Apache License, Version 2.0
## Technologies
- REST
- Microsoft Azure
- Service Bus
- HttpClient
## Topics
- Service Bus
## Updated
- 07/03/2014
## Description

<h1>Introduction</h1>
<div>This sample demonstrates how to send messages to and receive messages from Service Bus via HTTP/REST. It demonstrates the use of broker and custom message properties, the Send Batch feature, abando, lock renewal, and how to receive messages via HTTP that
 were sent via SBMP and vice versa. The sample consists of two projects: The first one uses the .NET WebClient and a synchronous programming model, the second one uses the .NET HttpClient and an asynchronous programming model.</div>
<p>&nbsp;</p>
<h1>Building the Sample</h1>
<ul>
<li>Install Windows Azure SDK 1.8 or later. </li><li>Create a Service Bus namespace. </li><li>In files Program.cs, replace strings ServiceBusNamespace, NamespaceOwner and NamespaceKey with the information of your namespace.
</li><li>Build the sample. Open the solution in Visual Studio 2010 or later and press F6.
</li><li>Press F5 to run the client. </li></ul>
<h1>Prerequisites</h1>
<ul>
<li>.NET Framework 4.5 (needed for HttpClient project only), .NET Framework 4.0&nbsp;
</li><li>Visual Studio 2012 (needed for HttpClient project only), Visual Studio 2010&nbsp;
</li></ul>
<h1>Description</h1>
<div>This sample implements two clients that send messages to and receive messages from Service Bus via HTTP. Both clients perform the following tasks:</div>
<ol>
<li>Create a SAS token or obtain a token from ACS. </li><li>Create a topic if it does not exist. </li><li>Create a subscription on the topic if it does not exist. </li><li>Send a message m1 with standard and custom properties. </li><li>Receive and process message m1. The message is destructively received (ReceiveAndDelete).
</li><li>Send a scheduled message m2. </li><li>Receive and process message m2. The message is not deleted (PeekLock). </li><li>Abandon message m2, receive it again, extend the lock, and then delete message m2.
</li><li>Send a batch of three messages. </li><li>Receive three messages. </li><li>Create a brokered message, send it via the SBMP protocol. Then receive the message via HTTP.
</li><li>Send a message via HTTP, then receive it as a brokered message via the SBMP protocol.
</li><li>Delete topic. </li></ol>
<div>This sample uses the API version string &quot;api-version=2012-03&quot;. This version works with Azure Service Bus and all versions of Service Bus for Windows Server. Later versions of the API don't work with Service Bus for Windows Server 1.0.</div>
<div>For Service Bus for Windows Server 1.0, the string apiversion can be set to &quot;&quot; or &quot;&amp;api-version=2012-03&quot;.</div>
<div>For Service Bus for Windows Server 1.1, the string apiversion can be set to &quot;&quot;, &quot;&amp;api-version=2012-03&quot;, &quot;&amp;api-version=2012-08&quot;,&quot;&amp;api-version=2013-04&quot;, or &quot;&amp;api-version=2012-07&quot;.</div>
<div>For Azure Service Bus the string apiversion can be set to &quot;&quot;, &quot;&amp;api-version=2012-03&quot;, &quot;&amp;api-version=2012-08&quot;,&quot;&amp;api-version=2013-04&quot;, &quot;&amp;api-version=2013-07&quot;, &quot;&amp;api-version=2013-08&quot;, &quot;&amp;api-version=2013-10&quot;, &quot;&amp;api-version=2014-01&quot;,
 or &quot;&amp;api-version=2014-05&quot;. As time progresses, additional API versions will become available for Azure Service Bus.</div>
<div>&nbsp;</div>
<div>The WebClient-based client constructs the HTTP request with the help of the WebClient class. Before sending the request, the client places a token in the web request&rsquo;s Authorization header. The client can manufacture its own SAS token, or it obtains
 a token from ACS. See <a href="http://code.msdn.microsoft.com/windowsazure/Service-Bus-HTTP-Token-38f2cfc5">
http://code.msdn.microsoft.com/windowsazure/Service-Bus-HTTP-Token-38f2cfc5</a> for different ways to authenticate with Azure Service Bus and Service Bus for Windows Server.</div>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">//&nbsp;Send&nbsp;Message.</span>&nbsp;
WebClient&nbsp;webClient&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;WebClient();&nbsp;
webClient.Headers[HttpRequestHeader.Authorization]&nbsp;=&nbsp;token;&nbsp;
webClient.Headers[HttpRequestHeader.ContentType]&nbsp;=&nbsp;<span class="cs__string">&quot;application/atom&#43;xml;type=entry;charset=utf-8&quot;</span>;&nbsp;
webClient.UploadData(address&nbsp;&#43;&nbsp;<span class="cs__string">&quot;/messages&quot;</span>&nbsp;&#43;&nbsp;<span class="cs__string">&quot;?timeout=60&quot;</span>,&nbsp;<span class="cs__string">&quot;POST&quot;</span>,&nbsp;message.body);&nbsp;
</pre>
</div>
</div>
</div>
<div>To create an entity, the client sends a PUT request to Service Bus to the entity URL that contains the entity description. If the entity description contains properties, these properties have to be specified in a specific order. For a queue, the order
 of settable properties is:</div>
<div>LockDuration<br>
MaxSizeInMegabytes<br>
RequiresDuplicateDetection<br>
RequiresSession<br>
DefaultMessageTimeToLive<br>
EnableDeadLetteringOnMessageExpiration<br>
DuplicateDetectionHistoryTimeWindow<br>
MaxDeliveryCount<br>
EnableBatchedOperations<br>
IsAnonymousAccessible<br>
Status<br>
ForwardTo<br>
UserMetadata<br>
SupportOrdering<br>
AutoDeleteOnIdle<br>
EnablePartitioning</div>
<div>For a topic, the order of settable properties is:</div>
<div>DefaultMessageTimeToLive<br>
MaxSizeInMegabytes<br>
RequiresDuplicateDetection<br>
DuplicateDetectionHistoryTimeWindow<br>
EnableBatchedOperations<br>
EnableFilteringMessagesBeforePublishing<br>
IsAnonymousAccessible<br>
Status<br>
UserMetadata<br>
SupportOrdering<br>
AutoDeleteOnIdle<br>
EnablePartitioning</div>
<div>For a subscription, the order of settable properties is:</div>
<div>LockDuration<br>
RequiresSession<br>
DefaultMessageTimeToLive<br>
DeadLetteringOnMessageExpiration<br>
DeadLetteringOnFilterEvaluationExceptions<br>
MaxDeliveryCount<br>
EnableBatchedOperations<br>
Status<br>
ForwardTo<br>
UserMetadata<br>
AutoDeleteOnIdle</div>
<div>Service Bus does not return an error if the order is violated. Instead, the property will appear twice in the entity description.</div>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">//&nbsp;Create&nbsp;topic.</span>&nbsp;
<span class="cs__keyword">byte</span>[]&nbsp;topicDescription&nbsp;=&nbsp;Encoding.UTF8.GetBytes(<span class="cs__string">&quot;&lt;entry&nbsp;xmlns='http://www.w3.org/2005/Atom'&gt;&lt;content&nbsp;type='application/xml'&gt;&quot;</span>&nbsp;
&nbsp;&nbsp;&#43;&nbsp;<span class="cs__string">&quot;&lt;TopicDescription&nbsp;xmlns:i=\&quot;http://www.w3.org/2001/XMLSchema-instance\&quot;&nbsp;xmlns=\&quot;http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\&quot;&gt;&quot;</span>&nbsp;
&nbsp;&nbsp;&#43;&nbsp;<span class="cs__string">&quot;&lt;DefaultMessageTimeToLive&gt;PT2M&lt;/DefaultMessageTimeToLive&gt;&quot;</span>&nbsp;
&nbsp;&nbsp;&#43;&nbsp;<span class="cs__string">&quot;&lt;MaxSizeInMegabytes&gt;2048&lt;/MaxSizeInMegabytes&gt;&quot;</span>&nbsp;
&nbsp;&nbsp;&#43;&nbsp;<span class="cs__string">&quot;&lt;/TopicDescription&gt;&lt;/content&gt;&lt;/entry&gt;&quot;</span>);&nbsp;
WebClient&nbsp;webClient&nbsp;=&nbsp;CreateWebClient(token);&nbsp;
webClient.UploadData(address&nbsp;&#43;&nbsp;<span class="cs__string">&quot;?timeout=60&quot;</span>,&nbsp;<span class="cs__string">&quot;PUT&quot;</span>,&nbsp;topicDescription);</pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<div>&nbsp;</div>
<div>Standard brokered message properties are defined in the BrokerProperties class, which is serialized into JSON format and added in the HTTP header
<em>BrokerProperties</em>.</div>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">//&nbsp;Serialize&nbsp;and&nbsp;add&nbsp;BrokerProperties.</span>&nbsp;
DataContractJsonSerializer&nbsp;serializer&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;DataContractJsonSerializer(<span class="cs__keyword">typeof</span>(BrokerProperties));&nbsp;
MemoryStream&nbsp;ms&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;MemoryStream();&nbsp;
serializer.WriteObject(ms,&nbsp;message.brokerProperties);&nbsp;
webClient.Headers.Add(<span class="cs__string">&quot;BrokerProperties&quot;</span>,&nbsp;Encoding.UTF8.GetString(ms.ToArray()));&nbsp;
ms.Close();</pre>
</div>
</div>
</div>
<div></div>
<div><span>Custom message properties are defined as a set of key-value pairs.
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">//&nbsp;Add&nbsp;custom&nbsp;properties.</span>&nbsp;
NameValueCollection&nbsp;customProperties;&nbsp;
customProperties.Add(<span class="cs__string">&quot;Priority&quot;</span>,&nbsp;<span class="cs__string">&quot;High&quot;</span>);&nbsp;
customProperties.Add(<span class="cs__string">&quot;Customer&quot;</span>,&nbsp;<span class="cs__string">&quot;12345&quot;</span>);&nbsp;
webClient.Headers.Add(customProperties);&nbsp;
</pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
To send a batch of messages, the body, brokered properties and user properties&nbsp;of all messages must be JSON-encoded and placed into the body of the HTTP message. The Content-Type of the HTTP request must be set to &ldquo;application/vnd.microsoft.servicebus.json&rdquo;.
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp">WebClient&nbsp;webClient&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;WebClient();&nbsp;
webClient.Headers[HttpRequestHeader.Authorization]&nbsp;=&nbsp;token;&nbsp;
webClient.Headers[HttpRequestHeader.ContentType]&nbsp;=&nbsp;<span class="cs__string">&quot;application/vnd.microsoft.servicebus.json&quot;</span>;&nbsp;
<span class="cs__keyword">string</span>&nbsp;msg1&nbsp;=&nbsp;<span class="cs__string">&quot;{\&quot;Body\&quot;:\&quot;This&nbsp;is&nbsp;the&nbsp;first&nbsp;message.\&quot;,\&quot;BrokerProperties\&quot;:{\&quot;Label\&quot;:\&quot;M1\&quot;}}&quot;</span>;&nbsp;
<span class="cs__keyword">string</span>&nbsp;msg2&nbsp;=&nbsp;<span class="cs__string">&quot;{\&quot;Body\&quot;:\&quot;This&nbsp;is&nbsp;the&nbsp;second&nbsp;message.\&quot;,\&quot;BrokerProperties\&quot;:{\&quot;Label\&quot;:\&quot;M2\&quot;},\&quot;UserProperties\&quot;:{\&quot;Priority\&quot;:\&quot;Low\&quot;}}&quot;</span>;&nbsp;
ServiceBusHttpMessage&nbsp;sendMessage&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;ServiceBusHttpMessage();&nbsp;
webClient.UploadData(address&nbsp;&#43;&nbsp;<span class="cs__string">&quot;/messages&quot;</span>&nbsp;&#43;&nbsp;<span class="cs__string">&quot;?timeout=60&quot;</span>&nbsp;&#43;&nbsp;ApiVersion,&nbsp;<span class="cs__string">&quot;POST&quot;</span>,&nbsp;Encoding.UTF8.GetBytes(<span class="cs__string">&quot;[&quot;</span>&nbsp;&#43;&nbsp;msg1&nbsp;&#43;&nbsp;<span class="cs__string">&quot;,&quot;</span>&nbsp;&#43;&nbsp;msg2&nbsp;&#43;&nbsp;<span class="cs__string">&quot;]&quot;</span>);</pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
If HTTP is used to receive a message that was sent via SMBP, the message body must be serialized with a custom serializer. In this snipped the message body is of type string.
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">//&nbsp;Send&nbsp;message&nbsp;via&nbsp;SMBP&nbsp;so&nbsp;that&nbsp;it&nbsp;can&nbsp;be&nbsp;received&nbsp;via&nbsp;HTTP.</span>&nbsp;
DataContractSerializer&nbsp;serializer&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;DataContractSerializer(<span class="cs__keyword">typeof</span>(<span class="cs__keyword">string</span>));&nbsp;
MemoryStream&nbsp;ms&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;MemoryStream();&nbsp;
serializer.WriteObject(ms,&nbsp;<span class="cs__string">&quot;This&nbsp;is&nbsp;a&nbsp;message.&quot;</span>);&nbsp;
ms.Flush();&nbsp;
ms.Seek(<span class="cs__number">0</span>,&nbsp;SeekOrigin.Begin);&nbsp;
BrokeredMessage&nbsp;sendMessage&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;BrokeredMessage(ms,&nbsp;<span class="cs__keyword">false</span>);&nbsp;<span class="cs__com">//&nbsp;The&nbsp;second&nbsp;parameter&nbsp;is&nbsp;required&nbsp;to&nbsp;ensure&nbsp;that&nbsp;the&nbsp;first&nbsp;parameter&nbsp;is&nbsp;correctly&nbsp;interpreted&nbsp;as&nbsp;a&nbsp;stream.&nbsp;It&nbsp;can&nbsp;be&nbsp;omitted&nbsp;when&nbsp;using&nbsp;later&nbsp;versions&nbsp;of&nbsp;the&nbsp;Service&nbsp;Bus&nbsp;client&nbsp;library.</span>&nbsp;
queueClient.Send(sendMessage);&nbsp;
ms.Close();&nbsp;
&nbsp;
<span class="cs__com">//&nbsp;Use&nbsp;HTTP&nbsp;to&nbsp;receive&nbsp;message&nbsp;that&nbsp;was&nbsp;sent&nbsp;via&nbsp;SBMP.</span>&nbsp;
<span class="cs__keyword">byte</span>[]&nbsp;body&nbsp;=&nbsp;webClient.UploadData(address&nbsp;&#43;&nbsp;<span class="cs__string">&quot;/messages/head?timeout=60&quot;</span>&nbsp;&#43;&nbsp;ApiVersion,&nbsp;<span class="cs__string">&quot;DELETE&quot;</span>,&nbsp;<span class="cs__keyword">new</span>&nbsp;<span class="cs__keyword">byte</span>[<span class="cs__number">0</span>]);&nbsp;
DataContractSerializer&nbsp;deserializer&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;DataContractSerializer(<span class="cs__keyword">typeof</span>(<span class="cs__keyword">string</span>));&nbsp;
<span class="cs__keyword">using</span>&nbsp;(MemoryStream&nbsp;ms&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;MemoryStream(message.body))&nbsp;
{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">string</span>&nbsp;body&nbsp;=&nbsp;(<span class="cs__keyword">string</span>)deserializer.ReadObject(ms);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Console.WriteLine(<span class="cs__string">&quot;Body:&nbsp;&quot;</span>&nbsp;&#43;&nbsp;body);&nbsp;
}</pre>
</div>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<div>
<div>
<h1>Source Code Files</h1>
HTTP Client (HttpClient)
<ul>
<li>Program.cs &ndash; Implements a client that sends and receives messages. </li><li>HttpClientHelper.cs &ndash; Implements helper routines that generate HTTP web requests and process HTTP web responses using the HttpClient class.
</li><li>BrokerProperties &ndash; Implements the standard properties of a Service Bus brokered message.
</li><li>ServiceBusHttpMessage &ndash; Implements a message class. </li></ul>
HTTP Client (WebClient)
<ul>
<li>Program.cs &ndash; Implements a client that sends and receives messages. </li><li>WebClientHelper.cs &ndash; Implements helper routines that generate HTTP web requests and process HTTP web responses using the WebClient class.
</li><li>BrokerProperties &ndash; Implements the standard properties of a Service Bus brokered message.
</li><li>ServiceBusHttpMessage &ndash; Implements a message class. </li></ul>
<h1>More Information</h1>
For more information on the Service Bus HTTP API, see <a href="http://msdn.microsoft.com/en-us/library/hh780717.aspx">
http://msdn.microsoft.com/en-us/library/hh780717.aspx</a>. For more information on Service Bus authorization and token providers via HTTP, see
<a href="http://code.msdn.microsoft.com/windowsazure/Service-Bus-HTTP-Token-38f2cfc5">
http://code.msdn.microsoft.com/windowsazure/Service-Bus-HTTP-Token-38f2cfc5</a>.</div>
</div>
</span>
<div>
<div></div>
<div></div>
<div class="endscriptcode">&nbsp;</div>
</div>
</div>
