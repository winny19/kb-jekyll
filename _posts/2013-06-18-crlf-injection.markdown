---
layout: post
title:  "CRLF Injection"
date:   2013-06-18 13:59:40
categories: vulnerabilities
---

CRLF Injection attacks and HTTP Response Splitting

The CRLF Injection Attack (sometimes also referred to as HTTP Response Splitting) is a fairly simple, yet extremely powerful web attack.  Hackers are actively exploiting this web application vulnerability to perform a large variety of attacks that include XSS cross-site scripting, cross-user defacement, positioning of client's web-cache, hijacking of web pages, defacement and a myriad of other related attacks.  A number of years ago a number of CRLF injection vulnerabilities were also discovered in Google’s Adwords web interface.

Sounds scary to you? You bet. Are you vulnerable? Quite possibly, and this is why.

Cause :CRLF Injection Mechanism

CRLF (Carriage Return and Line Feed) is a very significant sequence of characters for programmers. These two special characters represent the End Of Line (EOL) marker for many Internet protocols, including, but not limited to MIME (e-mail), NNTP (newsgroups) and more importantly HTTP.  When programmers write code for web applications they split headers based on where the CRLF is found. If a malicious user is able to inject his own CRLF sequence into an HTTP stream, he is able to maliciously control the way a web application functions.

A simple CRLF Injection example :

Suppose you run a vulnerable website that has a member section. An attacker will send an email to one of your members containing a CRLF-crafted link. This link appears to be legitimate; after all it points to your own website.  The link might look something like the one below:

http://www.yoursite.com/somepage.php?page=%0d%0aContent-Type: text/html%0d%0aHTTP/1.1 200 OK%0d%0aContent-Type: text/html%0d%0a%0d%0a%3Chtml%3EHacker Content%3C/html%3E

When the victim clicks on the link he will be served with the following HTML page:

<html>Hacker Content</html>

This attack appears to simply show the words "Hacker Content" on the victim's machine however the danger is that YOUR server has generated this HTML code, so effectively the hacker has injected HTML code into the victims browser via YOUR web server! Ouch.  More sophisticated variations of this example can lead to poisioning of the client's web-cache, cookies, XSS, temporary or permanent defacement of web pages and even information theft.

Example insight

If you look closely at the malicious URL you might notice a few occurences of the pattern %0d%0a. This pattern is the HTTP equivalent of CRLF and is the reason why we call this technique it a CRLF Injection Attack.

Known countermeasures

The only effective countermeasure is to properly sanitize URLs that point to web pages on your site containing any server re-direction code. Finding these holes is not a trivial task; most web applications today are littered with server-side redirects so the location of these vulnerabilities is not always clear, and it is very easy to miss most of them. Normally it can take hundreds of man-hours to test all your web page redirects and therefore it is very common to use an automated tool such as a web vulnerability scanner to find such web vulnerabilities.