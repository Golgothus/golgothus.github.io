# Identity and Access Management

## Reviews

- Various paremeters for context-based authentication
- Security issues and best practices for using common authentication protocols
- Security issues with various components of the network environment
- Commonly used exploits against authentication and access systems

Authentication falls into 3 categories:

- Something you know
  - Passwords / pins
- Something you have
  - Yubikey, Smartcard reader
- Something you do (or are)
  - Biometrics, handwriting / speech, typing pattern

## Endpoints

### Servers

Kerberos, has three key components that are use to challenge during a request for access:

- Authentication Service (AS)
- Ticket Granting Server (TGS)
- Key Distribution Center (KDC)

Ticket Granting Ticket - has information about the client, a timestamp and a copy of the TGS session key

This message is then encrypted with a symmetrical key by the TGS. The other message has a copy of the TGS session key. This message is encrypted with the user's secret key, so if the user is not in posession of the key, the client will be unable to read the TGS session key

![Kerberos Distribution](/images/2020-05-18%2013_02_09-Window.png)

Timing is a critical component to successful Kerberos authentication

> Maximum tolerance for computer clock synchronization - best practice dictates that this valude doesn't exceed five minutes

### Services

Masquerading as a service is a great way to phish users. To prevent abuse by rogue services, Microsoft's .NET Framework has a feature called Service Identity and Authentication. The Windows Communication Foundation infrastructure will ensure that the identity value of the requested service matches a preset value

- Standard authentication occurs first
- Secoondary authentication will then occur on the client machine, attempting to compare a stored value to the *endpoint identity* (comparing the value provided by the service during interaction to the stored value)

![Endpoint Identity](/images/2020-05-18%2022_07_26-Zachary's%20Kindle%20for%20PC%204%20-%20CompTIA%20CySA+%20Cybersecurity%20Analyst%20Certification%20Bu.png)

Elements to match by include:

- Certificates
- DNS
- RSA Value
- Service names
- User name

### Active Directory

Each item in AD is known as an *object*, each object has properties assigned to it called *attributes*

> The goal of many attackers is to gain a foothold in a network, pivot across systems, and eventually gain acces to the Active Directory DC (Domain Controller), thus allowing an attacker complete control of the environment

It is best to enact the policy of "least privilege", meaning to say providing permissions to only those who explicitly need it. AD comes with several administrator level accounts. It is best to only use the necessary number of admins for the network:

- Enterprise Admins (EA)
- Domain Admins (DA)
- Built-in Admins (BA)

A good way to track malicious use of the admin accounts is by using **Object Access Auditing**, this will assist in finding out what object was accessed / edited

### LDAP

TLDR; Limit use of LDAP queries, as users by default may be able to query the LDAP server to get more information than they may need

### TACACS+

Terminal Access Controller Access Control System Plus, is an authentication, authorization and accounting (AAA) protocol originated by CISCO (this is an alternative to Kerberos). Uses the client/server approach to determine the access level someone should have.

> Susceptible to birthday attacks, as the protocol uses TCP to enforce its policies. This can result in hash collisions

### RADIUS

Remote authentication dial-in user service is similar to TACACS+ wherein it is *also* AAA protocol, however unlike TACACS+, RADIUS does *not* encrypt both the Username / password. RADIUS also uses UDP (which makes this protocol susceptible to spoofing attempts), RADIUS is also susceptible to buffer overflow attacks

### SSO (Single Sign-on) and Federated sign-ons

SAML (Security Assertion Markup Language) is used widely within SSO. SAML provides acess and authorization between:

- A user
- Identity Provider (IDP)
- Service Provider (SP)

In SSO the critical asset to protect is the authentication mechanism. Compromise of the authentication mechanism could mean the loss or unavailability of access

## Chapter Review

1. A. the one-time passcode used for authentication was incorrect
2. ~~D. Disable hyperlinks in e-mail messages~~ C. Complex passwords may improve security, but also increase likeliness users will reuse / write them down
3. C. The roles associated with the account may have been inappropriate
4. B. It uses symmetric enryption & C. It requires use of AS, KDC, TGS
5. A. They decrease the impact of compromised credentials
6. C. Man-in-the-middle
7. A. All objects and subjects in the domain are compromised
8. ~~A. The password would also have to be changed on the external server~~ D. Main disadvantage of kerberos is it centralizes all keys in the KDC. Any domain password changes and changes to secret keys will be available to the attacker.
9. ~~A. The remote user is an attacker who compromised the VPN server, pivoted to the workstation, and is now exfiltrating data~~ D. MITM - makes the most sense to conclude that the attacker leveraged compromised VPN creds and is now intercepting all of the workstation's traffic to and from the website
10. C. You can't reach any conclusions strictly from built-in tools because a rootkit could interfere with their outputs
