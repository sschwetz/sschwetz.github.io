---
categories: Knowledge-Base
layout: page
full-width: true

typora-copy-images-to: /assets/img/${filename}
typora-root-url: ../

lang: en-AU

thumbnail: /assets/logos/nsx-icon.png
cover-img: /assets/logos/nsx.png

title: NSX Edge Node Sizing Guide
subtitle:

author: Stephen Schwetz
tags: 
  - KB0007
  - NSX
  - Edge
  - Knowledge Base
  - KB
updated: 2023-10-07

---

| **Appliance Size**   | **Memory ** | **vCPU** | **Disk Space ** | **VM Hardware Version**           | **Notes**                                                    |
| -------------------- | ----------- | -------- | --------------- | --------------------------------- | ------------------------------------------------------------ |
| NSX Edge Small       | 4 GB        | 2        | 200 GB          | 11 or later (vSphere6.0 or later) | Proof-of-concept deployments only. <br /><br />**Note:**L7 rules for firewall, load balancing and so on are not realized on a Tier-1 gateway if you deploy a small sized NSX Edge VM. |
| NSX Edge Medium      | 8 GB        | 4        | 200 GB          | 11 or later (vSphere6.0 or later) | Suitable when only L2 through L4 features such as NAT, routing, L4 firewall, L4 load balancer are required and the total throughput requirement is less than 2 Gbps. |
| NSX Edge Large       | 32 GB       | 6        | 200 GB          | 11 or later (vSphere6.0 or later) | Suitable when only L2 through L4 features such as NAT, routing, L4 firewall, L4 load balancer are required and the total throughput is 2 ~ 10 Gbps. It is also suitable when L7 load balancer, for example, SSL offload is required.See [Scaling Load Balancer Resources ](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-19B12230-8BF4-4AF7-9EB7-3701B0A0A439.html)in the NSX-T Data Center Administration Guide. For more information about what the different load balance sizes and NSX Edge form factors can support, see [https://configmax.vmware.com](https://configmax.vmware.com/). |
| NSX Edge Extra Large | 64 GB       | 8        | 200 GB          | 11 or later (vSphere6.0 or later) | Suitable when the total throughput required is multiple Gbps for L7 load balancer and VPN.See [Scaling Load Balancer Resources ](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/administration/GUID-19B12230-8BF4-4AF7-9EB7-3701B0A0A439.html)in the NSX-T Data Center Administration Guide. For more information about what the different load balance sizes and NSX Edge form factors can support, see [https://configmax.vmware.com](https://configmax.vmware.com/). |
