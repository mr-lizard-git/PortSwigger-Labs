# SQL Injection UNION Attack - Finding a Column Containing Text

## Platform
PortSwigger Web Security Academy

## Difficulty
Practitioner

## Category
SQL Injection

## Lab Objective
[ This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab.
The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results.
 To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data. ]

## Methodology
[First, we navigated to one of the sections within the online store, such as a product category page. We then copied the URL of that section and pasted it into the Hackbar Quantum Chrome extension.

Hackbar Quantum is a browser extension that makes it easier to modify URLs and insert SQL queries into URL parameters, rather than manually editing the URL in the browser’s address bar.

The first SQL injection payload we used was:

' UNION SELECT NULL,NULL --

We used this payload to identify the number of columns in the SQL query being executed by the website. If the website returned an error, we increased or decreased the number of NULL values and tested again until the error disappeared and the application returned a valid response.

By counting the number of NULL values required to obtain a successful response, we could determine the number of columns in the underlying SQL query.

Next, we used the following payload to identify which columns were capable of displaying string data:

' UNION SELECT 'A','B','C','D','E','F',NULL,NULL --

This payload allowed us to determine which columns in the query could return or display text-based values.

If the payload resulted in an error, we moved the string values (A, B, C, D, E, and F) to different column positions while keeping the remaining positions as NULL values.

For example, to test whether the second column could display string data, we used a payload in which the string values were placed in the second column:

' UNION SELECT NULL,'A',NULL --

By repeating this process and observing the website’s responses, we identified which columns were capable of displaying string-based data.]

## Payloads
[ The following SQL injection payloads were used during the testing process:

1. Identifying the Number of Columns

' UNION SELECT NULL,NULL --

Purpose: Used to determine the number of columns in the original SQL query by increasing or decreasing the number of NULL values until a valid response was received.

2. Identifying String-Compatible Columns

' UNION SELECT 'A','B','C','D','E','F',NULL,NULL --

Purpose: Used to identify which columns could accept and display string-based values in the application’s response.

3. Testing Different Column Positions

' UNION SELECT NULL,'A',NULL --

Purpose: Used to test whether a specific column position could display string data. The position of the string value was changed during testing to identify the appropriate output column.]

## Exploitation Steps
[ 1. Opened the online store and navigated to a product category page.
2. Copied the URL of the selected page and loaded it into the Hackbar Quantum Chrome extension.
3. Injected a UNION-based SQL injection test payload into the URL parameter.
4. Modified the number of NULL values to determine the number of columns in the underlying SQL query.
5. Once a valid response was obtained, the number of columns was identified.
6. Replaced the NULL values with different string values (A, B, C, D, E, and F) to identify which columns were capable of displaying text-based data.
7. Tested different column positions to determine which ones were reflected in the application’s response.

These steps were performed to investigate the possibility of exploiting a UNION-based SQL injection vulnerability.]

## Impact
[ If the application is vulnerable to UNION-based SQL Injection, an attacker may be able to manipulate SQL queries through vulnerable URL parameters.

Depending on the database configuration and the application’s security controls, this vulnerability could potentially allow an attacker to:

* Retrieve sensitive information from the database.
* Access unauthorized user information, including personal data.
* Extract database contents, such as usernames, email addresses, and other stored records.
* Bypass certain application-level access restrictions.
* Compromise the confidentiality and integrity of stored data.

The severity of the vulnerability depends on the database permissions, the type of information stored, and the level of access available to an attacker.]

## Remediation
- Use prepared statements.
- Use parameterized queries.
- Avoid dynamic SQL queries.

## Evidence
See screenshots directory.

## References
https://portswigger.net/web-security/sql-injection
