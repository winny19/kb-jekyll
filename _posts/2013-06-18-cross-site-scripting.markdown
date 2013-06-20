---
layout: post
title:  "Cross-site Scripting"
date:   2013-06-18 13:59:40
categories: vulnerabilities
---

Cross Site Tracing (XST) enables an attacker to steal the victim’s session cookie and possibly other authentication credentials transmitted in the header of the HTTP request when the victim’s browser communicates to destination system’s web server. The attacker first gets a malicious script to run in the victim’s browser that induces the browser to initiate an HTTP TRACE request to the web server. If the destination web server allows HTTP TRACE requests, it will proceed to return a response to the victim’s web browser that contains the original HTTP request in its body. The function of HTTP TRACE, as defined by the HTTP specification, is to echo the request that the web server receives from the client back to the client. Since the HTTP header of the original request had the victim’s session cookie in it, that session cookie can now be picked off the HTTP TRACE response and sent to the attacker’s malicious site. XST becomes relevant when direct access to the session cookie via the “document.cookie” object is disabled with the use of httpOnly attribute which ensures that the cookie can be transmitted in HTTP requests but cannot be accessed in other ways. Using SSL does not protect against XST.

If the system with which the victim is interacting is susceptible to XSS, an attacker can exploit that weakness directly to get his or her malicious script to issue an HTTP TRACE request to the destination system’s web server. In the absense of an XSS weakness on the site with which the victim is interacting, an attacker can get the script to come from the site that he controls and get it to execute in the victim’s browser (if he can trick the victim’s into visiting his malicious website or clicking on the link that he supplies). However, in that case, due to the single origin policy protection mechanism in the browser, the attacker’s malicious script cannot directly issue an HTTP TRACE request to the destination system’s web server because the malicious script did not originate at that domain. An attacker will then need to find a way to exploit another weakness that would enable him or her to get around the single origin policy protection.

A Cross-Site Tracing (XST) attack involves the use of Cross-site Scripting (XSS) and the TRACE or TRACK HTTP methods. According to RFC 2616, “TRACE allows the client to see what is being received at the other end of the request chain and use that data for testing or diagnostic information.”, the TRACK method works in the same way but is specific to Microsoft’s IIS web server. XST could be used as a method to steal user’s cookies via Cross-site Scripting (XSS) even if the cookie has the “HttpOnly” flag set and/or expose the user’s Authorization header.

The TRACE method, while apparently harmless, can be successfully leveraged in some scenarios to steal legitimate users’ credentials. This attack technique was discovered by Jeremiah Grossman in 2003, in an attempt to bypass the HttpOnly tag that Microsoft introduced in Internet Explorer 6 sp1 to protect cookies from being accessed by JavaScript. As a matter of fact, one of the most recurring attack patterns in Cross Site Scripting is to access the document.cookie object and send it to a web server controlled by the attacker so that he/she can hijack the victim’s session. Tagging a cookie as HttpOnly forbids JavaScript to access it, protecting it from being sent to a third party. However, the TRACE method can be used to bypass this protection and access the cookie even in this scenario.

Modern browsers now prevent TRACE requests being made via JavaScript, however, other ways of sending TRACE requests with browsers have been discovered, such as using Java.



####Examples

An example using cURL from the command line to send a TRACE request to a web server on the localhost with TRACE enabled. Notice how the web server responds with the request that was sent to it.

{% highlight sh %}
$ curl -X TRACE 127.0.0.1
TRACE / HTTP/1.1
User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
Host: 127.0.0.1
Accept: */*
{% endhighlight %}

In this example notice how we send a Cookie header with the request and it is also in the web server’s response.

{% highlight sh %}
$ curl -X TRACE -H "Cookie: name=value" 127.0.0.1
TRACE / HTTP/1.1
User-Agent: curl/7.24.0 (x86_64-apple-darwin12.0) libcurl/7.24.0 OpenSSL/0.9.8r zlib/1.2.5
Host: 127.0.0.1
Accept: */*
Cookie: name=value
{% endhighlight %}

In this example the TRACE method is disabled, notice how we get an error instead of the request we sent.

{% highlight html %}
$ curl -X TRACE 127.0.0.1
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>405 Method Not Allowed</title>
</head><body>
<h1>Method Not Allowed</h1>
<p>The requested method TRACE is not allowed for the URL /.</p>
</body></html>
{% endhighlight %}

Example JavaScript XMLHttpRequest TRACE request. In Firefox 19.0.2 it will not work and return a “Illegal Value” error. In Google Chrome 25.0.1364.172 it will not work and return a “Uncaught Error: SecurityError: DOM Exception 18” error. This is because modern browsers now block the TRACE method in XMLHttpRequest to help mitigate XST.

{% highlight js %}
<script>
  var xmlhttp = new XMLHttpRequest();
  var url = 'http://127.0.0.1/';

  xmlhttp.withCredentials = true; // send cookie header
  xmlhttp.open('TRACE', url, false);
  xmlhttp.send();
</script>
{% endhighlight %}