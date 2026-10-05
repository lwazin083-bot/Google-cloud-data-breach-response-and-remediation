 
<h1>Google Cloud data breach response and remediation</h1>


<h2>Description</h2>

A walk-through of common attacks techniques targeting Entra ID identities and what they leave behind in logs and to hunt for them in a SIEM environment like Splunk.
<br />


<h3>Password-based attacks</h3>
Attackers acquire credentials from credential dumping sites, and many of these credentials are tied to active corporate accounts where users have reused passwords. Once attackers has a list of credentials they attempt to gain access without needing to touch the target's network perimeter. A successful login is identical to a legitimate one, no exploit, no malware, no network anomalies. So, to catch such attacks we have to hunt for them and analyse the logs.


<br />
<br />

<p align="center">
Filtering for failed sign-in attempt</b>: <br/>
<img src="https://i.imgur.com/bDSjgFz.png" height="80%" width="80%"/>
<br />
<br />
Looking at IP Address "94.20.222.248", we have <b>Password spraying(T1110.003) :</b> Attackers use a small list of commonly used passwords against many different accounts, this is done so as not to trigger the lockout threshold. <br/>
<img src="https://i.imgur.com/OpAh9rJ.png" height="80%" width="80%"/>
<br />
<br />
List failed sign-in attempts by IP address:  <br/>
<img src="https://i.imgur.com/eEkwgh9.png" height="80%" width="80%"/>
<br />
<br />
Successful logins by user: <br/>
<img src="https://i.imgur.com/eEkwgh9.png" height="80%" width="80%"/>
<br />
<br />
successful logins by IP address--IP Address "38.165.231.218", supposed to be in Mexico, was finally successful in logging in linked to user "amanda.costa@finegalo.thm":  <br/>
<img src="https://i.imgur.com/EBJshP3.png" height="80%" width="80%"/>
<br />
<br />
<h3>MFA Bypass</h3>
MFA is the most impactful control against password based attacks. It provides a second layer in the verification of user identity beyond username and password. Unfortunately, attackers can, however unlikely, bypass MFA. <b>MFA Fatigue/Prompt Bombing (T1621)</b>--when attackers have valid credentials, they may initiate successive login attempts in order to bombard users with multiple MFA push notifications in the hope that their target will approve one, be it out of confusion, frustration, or the belief that the prompt is legitimate. 
<br />
<br />
<p align="center">
MFA failures by user:  <br/>
<img src="https://i.imgur.com/fDOS1Xt.png" height="80%" width="80%"/>
<br />
<br />
Successful sign-in activity :  <br/>
<img src="https://i.imgur.com/k7fnYap.png" height="80%" width="80%"/>
 <br />
 A clear MFA bypass pattern was found against a single user account: "igor.bicalho@finegal.thm". The attacker generated repeated MFA failures from a suspicious IP before ultimately achieving a successful authentication, indicating the MFA control was circumvented. 10 MFA failures were recorded against user "igor.bicalho@finegal.thm", all from IP "149.102.234.27". This is consistent with MFA fatigue. All failures came from Brazil(BR), occurring roughly every 6 minutes between 12:36 and 13:24 on 2026-03-04, the regular cadence may suggest automated tooling, not normal attempt. 7 successful logins recorded, 6 originated from IP "94.0.24.134" in Denmark(DK), spanning 2026-03-02 to 2026-03-04, consistent with user normal pattern. The one other successful login originates from the same IP responsible for all 10 failures minutes earlier. There is also a geographic anomaly in that there was a successful login in Brazil where the user's established pattern were all in Denmark. Recommendations: revoke the user's active sessions and reset credentials.
<br />
<br />

<h3>Privilege escalation and peristance</h3>
Post-compromise activities--once an attacker gains access into they seek to give to themselves higher privileges (expand their access) and establish persistence (make sure they can keep it). 
<br />
<br />
<p align="center">
Creating Backdoor Accounts:  <br/>
<img src="https://i.imgur.com/2HqNoCb.png" height="80%" width="80%"/>
<br />
<br />
Role Assignments:  <br/>
<img src="https://i.imgur.com/qVrjYtn.png" height="80%" width="80%"/>
<br />
<br />
Adding Alternate MFA Methods:  <br/>
<img src="https://i.imgur.com/6Ld2S6G.png" height="80%" width="80%"/>
<br />
A soon as our attacker, IP 149.120.234.27, bypassed the MFA control, they created an account-"rafael.michael@finegalo.thm". This classic backdoor creation. They want to ensure that they can survive a password reset on "igor.bicalho@finegal.thm". The newly created account was then escalated to the roles of "Global Administrator" and "Tenant Admin". 5 minutes later, "rafael.michael@finegalo.thm" performs MFA seeding, registering their own MFA. This was a tenant compromise: Credential theft --> MFA bypass --> Backdoor account creation --> Global Admin privilege --> MFA seeding --> Persistent access. 
Recommendations: disable both accounts at the centre of the compromise. Revoke all active sessions tenant-wide. Audit all role assignments made after 2026-03-04. Block the new MFA registration. Investigate possible data exiltration.

<br />
<br />

</p>

