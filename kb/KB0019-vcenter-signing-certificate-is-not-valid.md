---
categories: Knowledge-Base
layout: page
full-width: true

typora-copy-images-to: /assets/img
typora-root-url: ..

lang: en-AU

title: KB0019
subtitle: vCenter - Signing certificate is not valid
author: Stephen Schwetz
tags: 
  - KB0019
  - VCSA
  - ESX
  - vCenter
  - Knowledge Base
  - KB
updated: 2023-10-07


---

In an environment with a vCenter Server Appliance (VCSA) 6.5.x, 6.7.x or vCenter Server 7.0.x, you experience these symptoms:

* The vmware-vpxd service fails to start.

* Logging in to the vSphere Client fails with the error:

  ```
  HTTP Status 400 – Bad Request Message BadRequest, Signing certificate is not valid
  ```

* In the

   

  /var/log/vmware/vpxd-svcs/vpxd-svcs.log

   

  file, you see entries similar to:

  ```
  ERROR com.vmware.vim.sso.client.impl.SecurityTokenServiceImpl$RequestResponseProcessor opId=] Server rejected the provided time range. Cause:ns0:InvalidTimeRange: The token authority rejected an issue request for TimePeriod [startTime=Thu Jan 02 09:22:13 EST 2020, endTime=Fri Jan 03 09:22:13 EST 2020] :: Signing certificate is not valid at Thu Jan 02 09:22:13 EST 2020, cert validity: TimePeriod [startTime=Wed Jan 06 20:44:39 EST 2010, endTime=Wed Jan 01 20:54:23 EST 2020]
  ```

  Note

  : The

   

  **endTime**

   

  should be a date in the past if the certificate is expired.

* Logging in through the Web client display errors similar to:

  ![503 Service Unavailable (../../../Tools/Knowledge%2520Base/VMWare/vCentre/assets/rtaImage-20230919104754183.jpeg)](assets/rtaImage-20230919104754183.jpeg)

* Logging in through the Web Client displays a message similar to:

  ![User name and password are required](assets/img/rtaImage.jpeg)

* Replacing any certificate on either PSC or VCSA fails.

* Adding, modifying or deleting registrations from the Lookup Service manually using the [lsdoctor tool](https://kb.vmware.com/s/article/80469) fails.

* Deploying a new PSC and doing a cross-domain repoint fails.

* Deploying a new PSC as a replication partner on the existing SSO domain fails.

* Logging in through the Web client displays errors similar to:

  ```
  Cannot connect to vCenter Single Sign-On server https://VC_FQDN/sts/STSService/vsphere.local
  ```

  OR

```
Cannot connect to vCenter Single Sign-On server https://VC_FQDN:7444/sts/STSService/vsphere.local
```

OR

```
[400] An error occurred while sending an authentication request to the vCenter Single Sign-On server
```

* Connecting services with VCSA fails with vpxd authorization errors similar to:

  ```
  2020-10-07T08:27:51.547Z info vpxd[12853] [Originator@6876 sub=vpxCryptopID=SWI-7203af8f] Failed to read X509 cert; err: 151441516
  ```

* trying to export a VM as OVF fails, and

   

  /var/log/vmware/content-library/cls.log

  contains the following error:

  ```
  cls.log
  [2023-04-06T16:47:24.409+02:00] [INFO ] http-nio-5090-exec-1022      70179544 103561 ###### com.vmware.vise.security.spring.DefaultAuthenticationProvider     Session initialization complete for sessionId 103561, clientId 200264
  [2023-04-06T16:50:00.540+02:00] [INFO ] http-nio-5090-exec-1022       com.vmware.vapi.security.AuthenticationFilter                     Authentication failed com.vmware.vapi.std.errors.Unauthenticated: Unauthenticated (com.vmware.vapi.std.errors.unauthenticated) => {
          at com.vmware.cis.data.service.session.SessionAuthenticationHandler.authenticate(SessionAuthenticationHandler.java:36)
          at com.vmware.vapi.security.AuthenticationFilter.invoke(AuthenticationFilter.java:233)
  ```

**Note**: The preceding log excerpts are only examples. Date, time, and environmental variables may vary depending on your environment.



### Purpose

This article provide steps on regenerating and replacing expired Security Token Service (STS) certificate in VCSA 6.5.x, 6.7.x and vCenter Server 7.0.x using a shell script.

For steps on regenerating and replacing STS certificate in VMware vCenter Server 6.5.x and 6.7.x installed on Windows using a PowerShell script, see "[Signing certificate is not valid" error in vCenter Server 6.5.x and 6.7.x on Windows](https://kb.vmware.com/s/article/79263).

For more information on STS certificates, see Security [Token Service STS](https://docs.vmware.com/en/VMware-vSphere/6.0/com.vmware.vsphere.security.doc/GUID-18482A49-F9F8-4755-9113-3ADBAFE74EA3.html)



### Cause

These issue occurs when the Security Token Service (STS) certificate has expired. This causes internal services and solution users to not be able to acquire valid tokens and as a result fails to function as expected.

**Note**: When the STS certificate expires, it does so without warning. On some systems, this expiry may occur as soon as two years from the initial deployment.

The following scenarios can cause STS signing certificate to expire at 2 years:

* Fresh installation of PSC/vCenter Server 6.5 starting with U2 or later (6.5 only).
* Fresh installation of PSC/vCenter Server 6.5 U2 or any later 6.5 releases and upgraded to a later version including 6.7 and 7.0.
* STS signing certificate has been replaced using certool post-installation of PSC or vCenter Server.
* STS signing certificate has been replaced with custom certificate (Internal/External CA Signed).



### Impact / Risks

### Warning

This script interacts with the VMDIR's database. Take an offline snapshot concurrently for all vCenter Servers and Platform Service Controllers in the SSO domain before running the script. Failing to do so may result in an unrecoverable error and require redeploying vCenter Server.

**Notes**:

* This script should only be run once per SSO domain.
* In environments containing Horizon View, see [Connection Server unable to accept vCenter thumbprint with an error "There was an error identifying the validity of the server" (67701)](https://kb.vmware.com/s/article/67701).

### Resolution

To resolve the Signing certificate is not valid error:

1. Download the attached fixsts.sh script from this article and upload to the impacted PSC or vCenter Server with Embedded PSC to the /tmp folder.

2. If the connection to upload to the vCenter by the SCP client is rejected, run this from an SSH session to the vCenter:

   ```
   # chsh -s /bin/bash
   ```

3. Connect to the PSC or vCenter Server with an SSH session if you have not already per Step 2.

4. Navigate to the /tmp directory:

   ```
   # cd /tmp
   ```

5. make the file executable:

   ```
   # chmod +x fixsts.sh
   ```

6. Run the script:

   ```
   # ./fixsts.sh
   ```

7. Restart services on all vCenters and/or PSCs in your SSO domain by using below commands:

   ```
   # service-control --stop --all && service-control --start --all
   ```

   Note

   : Restart of services will fail if there are other expired certificates like Machine SSL or Solution User. Proceed with the next step to identify and replace expired certificates.

8. Check for expiration and replace any other expired certificates you might have, using certificate manager as shown in [How to use vSphere Certificate Manager to Replace SSL Certificates](https://kb.vmware.com/s/article/2097936) or follow Option 8 as shown in [How to regenerate vSphere 6.x certificates using self-signed VMCA](https://kb.vmware.com/s/article/2112283) if both Machine SSL and Solution User certificates are expired.

**Notes**:

* The following one-liner can determine other expired certificates for the vCenter Server Appliance:  

  ```
  for i in $(/usr/lib/vmware-vmafd/bin/vecs-cli store list); do echo STORE $i; sudo /usr/lib/vmware-vmafd/bin/vecs-cli entry list --store $i --text | egrep "Alias|Not After"; done
  ```

If you replaced Machine SSL or VMCA Root certificates, you must re-register 2nd party solutions such as NSX, SRM, and vSphere Replication. 

* If HLM (Hybrid Linked Mode) is in use without a gateway, you would need to re-sync the certs from Cloud to On-Prem after following this procedure.

The script will ask for the SSO administrator password and then regenerate and replace the STS certificate.

For example:

```
NOTE: This works on external and embedded PSCs
This script will do the following
1: Regenerate STS certificate
What is needed?
1: Offline snapshots of VCs/PSCs
2: SSO Admin Password
IMPORTANT: This script should only be run on a single PSC per SSO domain
==================================
Resetting STS certificate for vcsa1.gsslabs.org started on Fri May 22 14:39:40 UTC 2020

Detected DN: cn=vcsa1.gsslabs.org,ou=Domain Controllers,dc=vsphere,dc=local
Detected PNID: vcsa1.gsslabs.org
Detected PSC: vcsa1.gsslabs.org
Detected SSO domain name: vsphere.local
Detected Machine ID: ce510c87-35e6-444e-82f0-60a7527608a3
Detected IP Address: 192.168.0.51
Domain CN: dc=vsphere,dc=local
==================================
==================================

Detected Root's certificate expiration date: 2030 May 16
Detected today's date: 2020 May 22
==================================

Exporting and generating STS certificate

Status : Success
Using config file : /tmp/vmware-fixsts/certool.cfg
Status : Success

Enter password for administrator@vsphere.local:
Amount of tenant credentials: 1
Exporting tenant and trustedcertchain 1 to /tmp/vmware-fixsts

Deleting tenant and trustedcertchain 1

Applying newly generated STS certificate to SSO domain
adding new entry "cn=TenantCredential-1,cn=vsphere.local,cn=Tenants,cn=IdentityManager,cn=Services,dc=vsphere,dc=local"

adding new entry "cn=TrustedCertChain-1,cn=TrustedCertificateChains,cn=vsphere.local,cn=Tenants,cn=IdentityManager,cn=Services,dc=vsphere,dc=local"

Replacement finished - Please restart services on all vCenters and PSCs in your SSO domain
==================================
IMPORTANT: In case you're using HLM (Hybrid Linked Mode) without a gateway, you would need to re-sync the certs from Cloud to On-Prem after following this procedure
==================================
==================================
```


**Note**: If you receive the following error when trying to run the script:

```
bash: ./fixsts.sh: /bin/bash^M: bad interpreter: No such file or directory
```

This error is caused by DOS carriage returns added to the script when copying from a Windows-based text editor. To resolve this problem, run this command and rerun the script:

```
# sed -i -e 's/\r$//' fixsts.sh
```


**Notes**: 

* If you upgraded from vSphere 5.x or 6.0, there may have been multiple STS chains (trustedcertchain) present in TenantCredentials. When the nodes which issued the other chains are no longer accessible, this can cause authorization errors.
* Running the script will delete all the old STS chains and replace with a single new chain, resolving the authorization errors caused by the obsolete chains.
* If there are multiple certificate chains and no LEAF in the (trustedcertchain) please proceed to regenerate the STS Certificate.

### Related Information

* For more information on viewing the STS certificate and determining the expiry date, see [Checking Expiration of STS Certificate on vCenter Server](https://kb.vmware.com/s/article/79248).
* [VMware Skyline Health Diagnostics for vSphere - FAQ](https://kb.vmware.com/s/article/81931)
