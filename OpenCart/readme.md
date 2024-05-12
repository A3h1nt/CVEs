# OpenCart v4.0.2.3
### Description

The openCart project v4.0.2.3 contains a server side template injection vulnerability in it's edit theme functionality, which allows an authenticated admin user to execute arbitrary system commands, resulting in remote code execution. By default, the admin panel lacks any anti-bruteforce mechanism, making it easier for an attacker to bruteforce their way into the admin panel.

**Severity** : High

**CVSS Score** : 9.8

**CVSS Vector** : AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H

**Affected Version** : 4.0.2.3

**Reason** : 
1. Lack of anti-bruteforce mechanism on admin panel
2. Lack of Template Sandboxing and blacklisting of potentially dangerous template content.

### Proof Of Concept

1. Login as admin, intercept the request in burp and send it to intruder.

![brute-1](https://github.com/A3h1nt/CVEs/assets/56585189/70adbf43-abd8-45c1-8a41-201a02ce14d7)

2. Set the payload position to password and make some payload configurations.

![brute-2](https://github.com/A3h1nt/CVEs/assets/56585189/5061bf47-9314-43a6-8570-abf3261b7f5c)

3. Select payload list as simple list and select the any password wordlist.

![brute-3](https://github.com/A3h1nt/CVEs/assets/56585189/b4d77544-aa2e-4776-be18-10a5b567a484)

4. On the right iteration, the web application responds with a different content length and redirects user to the dashboard.

![brute-4](https://github.com/A3h1nt/CVEs/assets/56585189/00175b43-dcf4-4d81-b3bc-73591431a2be)

5. Once logged in, go to theme editor

![rce-1](https://github.com/A3h1nt/CVEs/assets/56585189/f169f182-e679-4ddf-8887-a8e5d2699316)

6. Click on add new

![rce-2](https://github.com/A3h1nt/CVEs/assets/56585189/ab4a74a1-1c04-4e23-b995-dc5627c03fa0)

7. Select a page that you want to edit (login page in this case).

![rce-3](https://github.com/A3h1nt/CVEs/assets/56585189/008dc859-0538-47af-b8db-74ba87b09252)

8. The theme editor uses twig template engine, meaning we can use twig template syntax payload, for starters `{{7*7}}`

![rce-4](https://github.com/A3h1nt/CVEs/assets/56585189/c6444206-a5a5-4b59-a1d5-1273c7e13f22)


9. Navigate to login page, we can see `49`, indicating the code is executed.

![rce-5](https://github.com/A3h1nt/CVEs/assets/56585189/f26e9c34-2eb1-464a-90d3-5efa4046ba53)


10. Similarly, we can use the `{{['hostname']|filter('system')}}` syntax, to execute system commands.

![rce-6](https://github.com/A3h1nt/CVEs/assets/56585189/8f819c1d-e268-47cd-8fcb-bfedd72cbf75)


