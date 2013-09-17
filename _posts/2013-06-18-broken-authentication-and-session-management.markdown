---
layout: post
title:  "Broken Authentication and Session Management"
date:   2013-06-18 13:59:40
categories: vulnerabilities
---

Vulenrabitlity type:Broken Authentication and Session Management

Description:How many of you have an idea about the above topic ‘Broken Authentication & Session Management’? If you not an I.T professional then you might be thinking its not for you, but folks, it is equally important for a developer as well as someone working in Human Resource department. How your simple ignorance can be a boon to a ‘Hacker’? 

How Broken Authentication and Session Management is a blessing to a Hacker?

Thief breaks into house which is least secured in the society. If you forgot to lock your doors properly before leaving then you just cant blame robbers. Simply imagine, what can happen if Master Key from the hotel gets misplaced. Something similar in IT world called as  ’Broken Authentication and Session Management’. It is vulnerabilities occurs when developers overlook certain security measures & fail to protect their users sensitive information such as user names, passwords, and session tokens

Cause: Some times developers simply write a code, test the desired result as per the requirement & forward the written program for testing without anticipating the flaws of written code. Tester believes that it is coming reliable source(Experience Developer), so he only test if the new password is working as desire. Hence this weakness can cause the threats to the system. Missed end to end test.
After code is written, Quality Analyst ensures the written code is well draft & as per the given requirement. Now, what if QA is not experience enough & just looking at the result, hand over the script to production. When the new password was created, did he checked if the old password is also working or not? Might the developer skip to lockout existing password.

Impact:All known web servers, application servers, and web application environments are susceptible to broken authentication and session management issues.

How to protect:
    	Use secure protocol (SSL/TLS) for all user management features including the critical business transactions supported by your website.
    
	Avoid using different HTML response in case of login failures due to different reasons. e.g. if password is wrong and user id is correct then don’t show a different message to user that may confirm him that user id is correct. For critical applications, avoid using email Id as user name

	Don’t completely trust on client side data and if possible always re-validate session data; Binding session id with IP address is one of validation practice but may impose certain limitations. For user management functions or critical transactions we should always reautehnticate user with secondary password otherwise with login password. For financial transactions always use out of bound channels (email/SMS) to notify users for critical events.

	Never store user password in the database in plan text. Use One Way Salted Hash to protect passwords from decryption. Also protect other critical data like secret keys or Answers to identify validation questions. The recommended minimum hash strength is SHA-256.
	
	Don’t use admin/root accounts for application authentication and go with least privilege policy; If possible segregate Admin site from normal user interface and if admin function is managed only by internal employees then restrict admin site access to internal network IPs.
	Change all default passwords and rename/disable default user names.

	Implement strong password policies but don’t force user to create very complicated passwords or forcing them to change passwords very frequently otherwise they may need to write down the passwords; If multiple websites are involved then use LDAP and SSO for centralized implementation and monitoring; Always ask for Old password whenever user is changing his password.
	
	Use HTTPOnly flag for cookie and encrypt sensitive data stored on client side; Also set the Domain and Path for cookie storing sensitive data so that cookie can’t be accessed across sub-domains.
	Don’t trust on sensitive data stored in Query String or hidden fields;

	Maintain minimum session timeout; Ensure Logout button is easily accessible. Use Alt tag if an image is used to show Logout text.

	Revalidate user session and role on every Request/GET and POST action for every critical webpage. Include authentication handshake process for Web Services and use secure protocols for critical web services and 3rd party integration.
	
	Never use Remember Me like feature for business critical applications and ensure that the browsers does not cache username & passwords. Critical web pages should be marked with “no cache” tag to prevent the usage of back button for resubmission of data.

	Use Out of bound channels for password recovery process but never include the password in email. Email should be used for triggering the process so that registered email account can be used for initiating password reset/recovery process. Include additional controls like asking secret key etc. before you allow password reset. Don’t allow to change email id or phone number during this process. Don’t lock an account or reset the existing password just when a Forget Password request is raised with a valid user id. Wait till the request is verified with out of bound channels or some other ways.

	Protect from other attacks especially from middle man attack, session fixation, SQL Injection (especially on Login interface), XSS, CSRF and Phishing attacks; Providing full protection for one threat is of no use if other attack surface can be easily exploited. Use infrastructure & networking layer protection for Brute Force attack and if possible use an Web Application Firewall to reduce other attack surface.

	Put account lockout features after max. 3-5 failed attempts and allow unlocking of account by revalidating of user from out of bound channels or other methods. Do notify user on critical events like password reset, account lockout, critical financial transactions etc. Please note that this feature may also be used by attacker for DoS attack in case attacker may get access to multiple valid user IDs.

	Use global exception handler for unhandled exception in the application code and create generic error pages for other website errors like “page not found”. Target is to ensure that an exception in system does not expose system configuration or any other details are part of exception handling.

Note: You may implement CAPTCHA for SPAM protection but CAPTCHA is not recommended and illegal in some countries to prohibit discrimination against disabled citizens. Similarly it is not recommended to rely on Question & Answers based on information that may be publicly available. For example someone working in an organization would have the email id, DOB, Father’s name and other demographic data available from his resume that is posted on a job portal or social web sites.