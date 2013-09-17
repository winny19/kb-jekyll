---
layout: post
title:  "SQL Injection"
date:   2013-06-18 13:59:40
categories: vulnerabilities
---

Vulenrability type : Critical

Vulnerability Name:The SQL Injection is used by malicious users to attack your application surface in legitimate manner to exploit weakness in the coding pattern especially how the application interact with the Database. The purpose of attack could be to get unauthorized access to sensitive data especially for other users/account and in some cases if such vulnerability exists then using attacks like Command Injections, the attacker may take complete control of system.

Common factors that make application vulnerable to SQL injection attacks are:

- Lack of proper input validations and trusting user supplied data
- Building dynamic SQL queries based on user supplied data;
- Application design issues like using URL parameters/hidden fields for backend operations or building dynamic SQL queries
- Running application code/services with database admin/high privileged accounts.

Example Attack Scenario:

	The application uses untrusted data in the construction of the following vulnerable SQL call:
String query = “SELECT * FROM accounts WHERE accountID=’” + request["id"] +”‘”;

	The attacker modifies the ‘id’ parameter in their browser to send: ‘ or ’1'=’1 as URL parameter (example.com/app/accountView?id=’ or ’1'=’1);
This changes the meaning of the query to return all the records from the accounts database, instead of only the intended customer’s data.

	Please note that just using stored procedures instead of dynamic SQL queries in application code does not prevent SQL Injection. If you use Stored Procedure parameters to execute directly at the backend that would also expose same vulnerability.
e.g. exec sp_executesql @user_supplied_param

	Many times attacker may not be successful in retrieving sensitive data by exploiting such vulnerabilities but he may still cause major damage by using SQL commands like DROP TABLE or mass change in sensitive data using DELETE or UPDATE statements.

Identify SQL Injection Vulnerabilities:

	Source code scanner or even a search utility can be very helpful in identifying vulnerable coding patterns on application code or in database layer. Most of Application Vulnerability Scanner including the free tools provide support to identify SQL Injection vulnerabilities.

Preventing SQL Injection:

Control Dynamic SQL: Ensure that dynamic SQL is not generated into the source code especially based on user controlled inputs. Be aware that user inputs may not be directly provided on attack surface e.g. while creating user profile one can enter malicious inputs for user profile fields, if data from any of these fields is used in your application that may cause SQL attacks. This is very common way of Cross Site Scripting attack.

Use Parameterized Queries: Most of platforms like .Net and Java supports parameterized queries to generate dynamic SQL. In most of platforms a parameter input is treated as a literal value and not treated as dynamic/executable code.

Java: select * from sometable where fieldA=? and FieldB=?

.NET: SqlDataAdapter objDA = new SqlDataAdapter(“select * from sometable where fieldA = @valA”, objConnection);
objCommand.SelectCommand.Parameters.Add(“@valA”, SqlDbType.VarChar, 20);
objCommand.SelectCommand.Parameters["@valA"].Value = WebForm_Name.Text;

In PHP use prepared statements with bound variables. They are provided by PDO, by MySQLi and by other libraries.

pg_query($conn, “PREPARE stmt_name (text) AS SELECT * FROM users WHERE name=$1?);
pg_query($conn, “EXECUTE stmt_name ({$name})”);
pg_query($conn, “DEALLOCATE stmt_name”);

Use Database Functions/Stored Procedures: Using Stored Procedures parameters for using with SQL queries is very straight forward and easy to use and dynamic SQL queries should only be used in application code when something is not feasible with Stored Procedures. Also make sure you don’t use database system provided functions like “exec sp_executesql @user_supplied_param”. Restrict access to critical functions like “xp_cmdshell” in SQL Server.

Use Web Application Firewall to create protection shield without any change in existing applications. Microsoft UrlScan may be used on Microsoft IIS and many other free & profession Security Tools are also available in market.

Use Least Privilege Access Never use system admin user accounts to provide connectivity between application code and database. Create different account groups for application connectivity with least privilege access policy. In case any part of implementation need higher privilege, separate such deployment and use a higher privilege account that should be used by limited application code.

Use Server Side TriggersDatabase triggers are mostly avoided due to performance reasons. But Triggers may provide good control to restrict mass changes/deletion for highly critical data tables.

Exception HandlingMake sure the application does not crash and expose critical information like database structure or configuration details due to an unhandled exception. Please also take measures to protect from Blind SQL Injection when an attacker does not get desired data by SQL Injection attack but use the exception incidents to validate his guess/assumptions about system.

Avoid Obvious NamesFor successful SQL Injection many time attacker need to guess the database structure. Hence avoid very obvious names in database structure for sensitive data. e.g. table names Customers, UserTable, Users etc. or column names like UserName, UserId, Password, SSN, CustomerEmail, Emailid etc.

Validate user Inputs: User input validation is required not only to prevent SQL Injection but also many other critical attacks like Cross Site Scripting. Validate data for type, length, format, and range. In case when you have no other option but to use dynamic SQL instead you must check parameter/inputs for such characters that have special meaning for database platform such as the single quote character in SQL Server. Test the content of string variables and accept only expected values. Reject entries that contain binary data, escape sequences, and comment characters. This can help prevent script injection and can protect against some buffer overrun exploits.

			Implement multiple layers of validation. Precautions you take against casually malicious users may be ineffective against determined attackers. A better practice is to validate input in the user interface and at all subsequent points where it crosses a trust boundary.
			
When you are working with XML documents, validate all data against its schema as it is entered.
Don’t rely on client side validations.Never concatenate user input that is not validated. String concatenation is the primary point of entry for script injection.