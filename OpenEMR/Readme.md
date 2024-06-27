# OpenEMR
*Broken Access Control*

[CVE-2024-37734](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-37734)

### Description

The openEMR project version 7.0.2 is affected by a broken access control vulnerability that allows an attacker to takeover any arbitrary user's private conversations using the `noteid` parameter in the message POST request, the attacker can essentially view, edit, post and delete messages.

**Severity** : High

**CVSS Score** : 8.8

**CVSS Vector** : AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H

**Fix** : https://github.com/openemr/openemr/pull/7435

**Download Patch For v7.0.2** : https://www.open-emr.org/wiki/index.php/OpenEMR_Patches

### Proof Of Concept

1. Login as an administrator user and create new message

![1](https://github.com/A3h1nt/CVEs/assets/56585189/9f5dc9ff-1afc-4e61-a06a-f03a6e1896d6)


2. Enter the `type`, `status`, `patient` and select the receiver of this message as `physician`.

![2](https://github.com/A3h1nt/CVEs/assets/56585189/86ec15de-1e1e-466e-9e32-bc455fe5618e)


3. On the physician message center, we can see the new message, click on this message and intercept the request using proxy.

![3](https://github.com/A3h1nt/CVEs/assets/56585189/5ae0dc6c-a946-4d17-bde1-305c2c0b34eb)


4. Take note of the noteid, `14` in this case and forward the request.

![4](https://github.com/A3h1nt/CVEs/assets/56585189/cb933271-a548-4a7b-b3d0-e8ad345bffe5)


5. We can see the received message on physician's message center, now add some comment and send the message back to the administrative user.

![5](https://github.com/A3h1nt/CVEs/assets/56585189/f7c1d30e-281f-47de-833c-6465a733bb01)


6. We can see the administrative user has successfully received the message.

![6](https://github.com/A3h1nt/CVEs/assets/56585189/6408e23d-4f87-4ade-9a70-50f650591ee1)


7. Now login as Clinician, go to Message Center > All Messages and try to view the message.

![7](https://github.com/A3h1nt/CVEs/assets/56585189/7ba3d3c5-e313-474c-a9c4-e5a70c0a597a)


8. We can see the web application denies access to view the message to user clinician.

![8](https://github.com/A3h1nt/CVEs/assets/56585189/6ec0c115-44c0-4244-8a41-1a4e1b34caed)


9. Logged in as user clinician, create a new message, fill the required details and intercept the request in proxy.

![9](https://github.com/A3h1nt/CVEs/assets/56585189/6727a39b-d0a7-4bb4-83bb-5b4020bcdf05)


10. We can see, the noteid parameter being present, but it doesn't contain any value yet.

![10](https://github.com/A3h1nt/CVEs/assets/56585189/e2ac7e44-43cf-4c8d-b1a4-878e52fe189f)


11. Change the value of noteid parameter to 14 ( 14 is the noteid of the message which was initially sent by admin to physician ).

![11](https://github.com/A3h1nt/CVEs/assets/56585189/84bb1483-dedb-4984-ac01-d65d33728aa0)


12. Once the request is submitted, we can see the message appear, if we try to view this message now.

![12](https://github.com/A3h1nt/CVEs/assets/56585189/3ded1819-d8c9-47d6-9c44-2129a31f6365)


13. We can see the access control has been bypassed and the user clinician is able to not only view the message that was not meant for him, but also make changes to it ( we can see the status is now changed to `read` )

![13](https://github.com/A3h1nt/CVEs/assets/56585189/82e6a0bf-f6fb-4f0a-8bbf-d39f5710aa56)


### Conclusion

In real world scenarios, a user can simply bruteforce the `noteid` parameter, in order to read all the existing messages on the website, the user can not only view, but edit and delete the messages too, which poses great risk to other users on the application.

-------------
### How to fix ?

A proper access control mechanism should be implemented to validate the `noteid` parameter, and a user should not be allowed to create new message with a `noteid` value of an existing message.
