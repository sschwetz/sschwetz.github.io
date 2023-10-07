---
categories: Knowledge-Base
layout: page
full-width: true

typora-copy-images-to: /assets/img/${filename}
typora-root-url: ../

lang: en-AU

title: Could not connect to one or more vCenter Server Systems - sdk error in the vSphere Client
subtitle:
author: Stephen Schwetz

cover-img: /assets/logos/esx.png
thumbnail: /assets/logos/exi-icon.png

tags: 
  - KB0020
  - ESX
  - vCenter
  - VCSA
  - Knowledge Base
  - KB

updated:
---

## Symptoms

* vCenter service becomes unresponsive randomly

* In the vpxd.log(/var/log/vmware/vpxd)file, you see entries similar to:

  ```text
  2020-04-14T12:59:58.853-06:00 error vpxd[10382] [Originator@6876 sub=HTTP session map] Out of HTTP sessions: Limited to 2000
  2020-04-14T12:59:58.869-06:00 error vpxd[10395] [Originator@6876 sub=HTTP session map] Out of HTTP sessions: Limited to 2000
  ```

* Session exhaust could be due to the following reasons:

  * Any external/internal solution trying to login to vCenter with incorrect credentials

  ```
  2020-04-14T12:48:16.306-06:00 info vpxd[10394] [Originator@6876 sub=Default opID=faf1555] [VpxLRO] -- ERROR lro-134510 -- SessionManager -- vim.SessionManager.impersonateUser: vim.fault.InvalidLogin:
  
   --> Result:
   --> (vim.fault.InvalidLogin) {
   --> faultCause = (vmodl.MethodFault) null,
   --> faultMessage = <unset>
   --> msg = ""
   --> }
   --> Args:
   -->
   --> Arg userName:
   --> "USERNAME"
   --> Arg locale:
   --> "en"  
  ```

  * Any service account user trying to access vCenter with insufficient privileges

  ```
  2020-06-09T20:33:42.481Z info vpxd[7FE11CC03700] [Originator@6876 sub=Default opID=68209c6d] [VpxLRO] --    ERROR lro-592994 -- SessionManager -- vim.SessionManager.login: vim.fault.NoPermission:
   --> Result:
   --> (vim.fault.NoPermission) {
   --> faultCause = (vmodl.MethodFault) null,
   --> faultMessage = <unset>,
   --> object = 'vim.Folder:3efc1455-d78f-4e78-b90c-32b43c02abc0:group-d1',
   --> privilegeId = "System.View"
  ```

## Cause

This issue can occur due to multiple failed login attempts exhausting the HTTP sessions( Usually Backup client, Monitoring client or any external solution integrated with VC with an expired password/account can cause excessive retry attempts, which can exhaust the available sessions)

## Resolution

To resolve the session exhaust situation, 

1. Identify if the respective solution account trying to log into the vCenter server with invalid credentials. (Address the respective account's issue by resetting/updating the password, etc.,)
1. Assess the session count, and if it is required to increase the number of sessions, follow the steps mentioned in the workaround section.

## Workaround

To work around this, Increase `<maxSessionCount>` in vpxd.cfg file.

* Take a backup of the vpxd.cfg from /etc/vmware-vpx/

* Stop the vpxd service with the command: `service-control --stop vmware-vpxd`

* Added the soap and maxsessioncount entries as per below:

  ```
  <config>
     ...
     <vmacore>
        ...
        <soap>
           ...
           <maxSessionCount>6000</maxSessionCount>
           ...
        </soap>
        ...
     </vmacore>
     ...
  </config>
  ```

* Start the vpxd service with the command: `service-control --stop vmware-vpxd`
