---
layout: post
title:  "XML Injection and XPath Injection"
date:   2013-06-18 13:59:40
categories: vulnerabilities
---
Vulnerability Type : High

Vulnerability Name : XML Injection and XPath Injection

Description:

	Most sites have a way to store data, the most common of which is a database. However some sites use XML to store data, and use a method of looking at the data known as XPath. When using SQL Databases to store site data, a site owner has to beware of SQL Injection attacks which can steal or alter data. Similarly, sites which store information using XML and XPath have to beware XPath injection to prevent data theft or modification.
	
XML and XPATH Injection

	Applications which return different results based on user input (such as logging a user in based on username/password) may not properly filter all user input.

	Imagine that you have user data stored in an XML file, and that XML file looks something like this:

<users>
	<user ID =1>
		<username>Admin</username>
		<password>MyPassw0rd</password>
		<role>admin</role>
	</userid>
</users>

	Looking at the code the site uses to log in, it dynamically builds an XPath query in PHP, after a user has given username and password in a form (with user and pass variables):

<?php
$login = simplexml_load_file("users.xml");
$result=$login->xpath("//User[username/test()='".$_POST['user']." AND password/text()='".$_POST['pass']."'";
?>
	
	The main problem above is that data from the user is not sanitized; the POST variable is being used to pull data directly from the login form, and placing it into the XPATH query. At this point, an attacker could try putting in a special string for username:

' or 1=1 or '

	Now the query always returns the first user (admin in our case) and the attacker is logged in as an administrator!

	Other attacks are possible using similar logic. Instead of logging in as an administrator, an attacker could use other special strings to request data from the document they should not have access to, potentially gathering all the data in the XML file.

How Does this Impact Security?

A successful attack would allow an attacker to steal the entire database, including all sensitive data, and log in with administrative privileges in the application.

This is one of the most high risk vulnerabilities an application can have.

Preventing XML Injection

	XPath injection can be prevented in a similar way to SQL Injection. The best way is to carefully sanitize user input. Any data received from a user should be considered unsafe. Removing all single and double quotes should remove most types of this kind of attack.

	Beware that removing quotes can have side effects, such as when usernames might contain valid quotes. Imagine common names such as O'Malley, which contains a legitimate quote and may be input on a name request form. In these cases, the input should be escaped, often by adding a "\" in front of quotes. Check with your specific library and XML software for the proper syntax.

	Most programming libraries contain functions to help escape user input for the sake of queries. Check your specific documentation for the correct way to automatically escape data for querying XML documents.
