# OpenCart
*Server Side Template Injection*

[CVE-2024-40420](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-40420)

### Description

The openCart project v4.0.2.3 contains a server side template injection vulnerability in it's edit theme functionality, which allows an authenticated admin user to execute arbitrary system commands, resulting in remote code execution. This happens due to lack of template sandboxing and blacklisting of potentially dangerous template content.

**Severity** : High

**CVSS Score** : 8.0

**CVSS Vector** : AV:N/AC:H/PR:H/UI:N/S:C/C:H/I:H/A:H


### Proof Of Concept

1. Once logged in as administrator, go to theme editor.

![rce-1](https://github.com/A3h1nt/CVEs/assets/56585189/a5127189-6c70-441a-b494-d05a49ad6209)

2. Click on add new

![rce-2](https://github.com/A3h1nt/CVEs/assets/56585189/8f3d98a1-d46c-41bd-a0ee-23cbe1a8a810)

7. Select a page that you want to edit (login page in this case).

![rce-3](https://github.com/A3h1nt/CVEs/assets/56585189/acde30aa-b3bf-4bf0-ac8d-6d5882d4a788)

8. The theme editor uses twig template engine, meaning we can use twig template syntax payload, for starters `{{7*7}}`

![rce-4](https://github.com/A3h1nt/CVEs/assets/56585189/a45f9a91-4edd-4d6b-b3be-6d30fc231f37)

9. Navigate to login page, we can see `49`, indicating the code is executed.

![rce-5](https://github.com/A3h1nt/CVEs/assets/56585189/a44a5e5f-dfea-47b9-bff5-eddabbd916ba)

10. Similarly, we can use the `{{['hostname']|filter('system')}}` syntax, to execute system commands.

![rce-6](https://github.com/A3h1nt/CVEs/assets/56585189/d5f8ffde-4ad5-4202-8df2-cc80784f56d3)

