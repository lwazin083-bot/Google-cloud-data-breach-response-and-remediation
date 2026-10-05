 
<h1>Google Cloud data breach response and remediation</h1>


<h2>Description</h2>

examining vulnerabilities and findings in Google Cloud Security Command Centre and taking remediation steps.
<br />


<br />
<br />

<p align="center">
Identified vulnerabilities related to the breach, these are misconfigurations by resource type</b>: <br/>
<img src="https://i.imgur.com/LJoWCkw.png" height="80%" width="80%"/>
<br />
<br />
In the Google Cloud Compliance section we view the details in PCI DSS 3.2.1 report. The Payment Card Industry Data Security Standard (PCI DSS) is a set of security requirements that organizations must follow to protect sensitive cardholder data. Complience with the PCI DSS must be ensured to protect cardholder data. <br/>
<img src="https://i.imgur.com/uQnzLbH.png" height="80%" width="80%"/>
<br />
<img src="https://i.imgur.com/ZEFZNYQ.png" height="80%" width="80%"/>
<br />
<img src="https://i.imgur.com/SYlrecB.png" height="80%" width="80%"/>
<br />
<img src="https://i.imgur.com/flLvtRw.png" height="80%" width="80%"/>
<br />
<br />
<ul>
  <li>Firewall rule logging should be enabled so we can be able to audit network access</li>
  <li>Firewall rules should not allow connections from all IP addresses on TCP or UDP port 3389</li>
  <li>Firewall rules should not allow connections from all IP addresses on TCP or SCTP port 22</li>
  <li>VMs should not be assigned public IP addresses</li>
  <li>Cloud Storage buckets should not be anonymously or publicly accessible</li>
  <li>VPC Flow logs should be Enabled for every subnet VPC Network</li>
  <li>Instances should not be configured to use the default service account with full access to all Cloud APIs</li>
 </ul>
<br />
<br />
<p align="center">
<b>Containment</b>: Findings indicated that a virtual machine was configured in a way that left it vulnerable to attack. To remediate, we shut down VM (cc-app-01):  <br/>
<img src="https://i.imgur.com/wfkynRX.png" height="80%" width="80%"/>
<br />
<br />
A new VM(cc-app-02) created from cc-app-01 snapshot: <br/>
<img src="https://i.imgur.com/xbzJ2Pi.png" height="80%" width="80%"/>
<br />
<br />
replacement VM is live but still require hardening:  <br/>
<img src="https://i.imgur.com/06jdumC.png" height="80%" width="80%"/>
<br />
<br />
<p align="center">
Hardening our VM :  <br/>
<img src="https://i.imgur.com/RBb7kQq.png" height="80%" width="80%"/>
<br />
<br />
Instance started again :  <br/>
<img src="https://i.imgur.com/p7o8djB.png" height="80%" width="80%"/>
<br />
<br />
<p align="center">
selecting and deleting the compromised VM:  <br/>
<img src="https://i.imgur.com/JeBiGlk.png" height="80%" width="80%"/>
<br />
<br />
Revoke public access to the storage bucket and switch to uniform bucket-level access control, significantly reducing the risk of data breaches.s:  <br/>
<img src="https://i.imgur.com/E4DtZYG.png" height="80%" width="80%"/>
<br />
<br />
Limit firewall ports access:  <br/>
<img src="https://i.imgur.com/KJBmnxp.png" height="80%" width="80%"/>
<br />
<br />

</p>

