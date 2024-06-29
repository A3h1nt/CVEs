### Description

The openCart project v4.0.2.3 contains a server side template injection vulnerability in it's edit theme functionality, which allows an authenticated admin user to execute arbitrary system commands, resulting in remote code execution. The issue essentially arises due to Lack of template sandboxing and blacklisting of potentially dangerous template content.

**Severity** : High

**CVSS Score** : 8.0

**CVSS Vector** : AV:N/AC:H/PR:H/UI:N/S:C/C:H/I:H/A:H

**Affected Version** : 4.0.2.3

### Proof Of Concept

1. Login as admin, intercept the request in burp and send it to intruder.

![[brute-1.png]]

2. Set the payload position to password and make some payload configurations.

![[brute-2.png]]

3. Select payload list as simple list and select the any password wordlist.

![[brute-3.png]]

4. On the right iteration, the web application responds with a different content length and redirects user to the dashboard.

![[brute-4.png]]

5. Once logged in, go to theme editor

![[rce-1.png]]

6. Click on add new

![[rce-2.png]]

7. Select a page that you want to edit (login page in this case).

![[rce-3.png]]

8. The theme editor uses twig template engine, meaning we can use twig template syntax payload, for starters `{{7*7}}`

![[rce-4.png]]

9. Navigate to login page, we can see `49`, indicating the code is executed.

![[rce-5.png]]

10. Similarly, we can use the `{{['hostname']|filter('system')}}` syntax, to execute system commands.

![[rce-6.png]]
