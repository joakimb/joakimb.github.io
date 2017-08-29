---
layout: post
title: "Security issues with cloud based enterprise communication"
date: 2017-08-04 12:00:00 +0200
---

## Introduction

Security is, and always has been, a basic requirement for quality products. It has been part of every serious system since the start of networked computing in the late 1900:s and is now part of every mans life with the moving of some parts of our lives to the internet. However, it has always been hidden under the surface. It has not been publicly advertised by neither the media or the system builders.

The situation has changed. Nowadays, security breaches is front page news on the most respected newspapers and system builders openly discuss their security model as a part of marketing.

## Secure enterprise communication

### The data

The first thing to realize when talking about security in enterprise communication is that you are dealing with sensitive data.
The messages and documents your employees send to each other very often contain things that are sensitive to either the company or the employee.
Obvious examples are: business plans, information acquired under NDA, maternity leaves and salary information.

This data is often not only embarrassing and damaging for business to leak, it is also often regulated in law.
GDPR and stock market information regulations comes to mind.

### How the data is secured

All security is based on conditionally selecting trusted parts of a system. To enforce the security - i.e. the exclusion of untrusted entities - we use the tools, rules and cryptography.
Rules are prone to human error, or malicious intents.
Cryptography on the other hand is highly resistant to these flaws.

#### Securing every part of the system

Most people that have come in contact with computer security have heard about the common techniques for connection security and end node security such as TLS and firewalls.

These technologies are very good technologies, but the purpose is to secure the infrastructure, piece by piece, or as we call it in the industry, __hop-by-hop__ security.
This means that when for example sending an email, the email is encrypted in transit and in storage, but the parties delivering the mail are able to decrypt the message.
This is a weakness.
There can be vulnerable parts of the chain; or some part of the chain can be malicious.
Traditional security models like this are good for securing infrastructure that you own, it is not designed to protect data which does not explicitly belong to the system owner.

#### Securing the data at the end nodes

When you do not control 100% of the infrastructure - as when using cloud services - or the data does not belong to the system - as in a chat service or a mail system, protecting the infrastructure is not enough.
Additionally, you need to secure the data itself.
If the data is encrypted at the message sender, and the decryption key is only made available to the intended recipient, you achieve what is called __end-to-end__ security (E2E).
This type of security eliminates the need to fully trust every part of the system, making for a reduced attack surface and brings data ownership back from the system to the data producer.

There are many technologies available for E2E, e.g. [HYKER](https://hyker.io), [PGP](http://openpgp.org), [Signal](https://whispersystems.org) and [OSCoAP](https://tools.ietf.org/html/draft-selander-ace-object-security-05). 
They all achieve fully secure E2E, but with important differences making them suited for different purposes.

## The Briteback security model

Briteback aims to be the optimal communication tool for enterprises, taking all of their needs into consideration.
A modern enterprise needs a smooth communication solution integrating several ways of communication.
It also needs the information shared via this tool to be fully secure from leaks and changes.
Confidentiality, integrity and authenticated origins of information are core values for enterprises.


### Briteback security model

For the above reasons, Briteback has integrated full end-to-end security in our services.

The chosen service for end-to-end security is [HYKER](https://hyker.io), a Swedish security company dealing exclusively with end-to-end cryptography.
The pros and cons of this security model is described in further detail below, but the key takeaways is that regardless of if any Briteback or HYKER services are hacked or malicious, the customer data remains fully out of reach for both attackers and Briteback and HYKER themselves.

This is a very strong security statement.
For an enterprise, it is highly preferable to having to trust the sincerity and continuous server maintenace of external organizations.

### Briteback competition security models


#### Microsoft Office 365

Microsoft uses the traditional security model of hop-by-hop encryption, separating connection security and security for data at rest.

A more detailed description is available at:
[Office 365 security](https://products.office.com/en/exchange/office-365-message-encryption)
[Office 365 security FAQ](https://technet.microsoft.com/sv-se/library/dn569285.aspx)

The gist of the content provided on those pages is that Microsoft can read your data, but that they are trustworthy and that you should therefore not care.
Since they know that they are in full control of their customers data, Microsoft issued this privacy statement:
[Privacy Statement](https://support.office.com/en-us/article/Office-365-Protected-Message-Viewer-Portal-privacy-statement-05b2e7e4-230e-48a9-802c-4cafac0d7c9d?ui=en-US&rs=en-US&ad=US)

The fact that you need to trust their privacy statement and do not have access to end-to-end encryption makes for a vulnerable security model.

This [video](https://www.youtube.com/watch?v=sAaO_maYowY) talks about that Microsoft is a supplier which you can trust. They mention encryption in transit and encryption at rest. They also say that they do not use your data, and that you __should not have to worry about that__. This begs the question: why does Microsoft not offer E2E encryption? 

With Microsofts security model, they are forcing their customers to worry about such things. They specifically state that Microsoft engineers are the only ones that have access to your data, no one else. Why should you need to trust the engineers at Microsoft? Microsofts entire security argument is based around "Don't trust anybody but us". It is the "but us" part that is weak. Strategies based on "don't trust anybody, not even us" are much stronger.

So dont trust words, trust actions. Trust proper encryption rather than privacy statements.

#### MS-teams
MS-teams is security-wise equivalent to Office 365. It is new set of new communication tools, but they are based upon Office 365.

This is stated in a quote: "Microsoft Teams is built upon Office 365 Groups and provides a new way to access shared assets for an Office 365 Group.", coming from their [FAQ](https://support.office.com/en-us/article/Frequently-asked-questions-about-Microsoft-Teams-–-Admin-Help-05cbe533-2181-4e95-a4b0-52cd7695fafca).


#### Slack

Slack boasts about their compliance certifications on their [security page](https://slack.com/security). They also talk about their various security features, such as SAML-based SSO. This is all good. Their Achilles' heel is the same as Microsofts though. They focus on hop-by-hop encryption through encryption in transit and encryption at rest. Slack offers no end-to-end encryption functionality.

So, security-wise, Slack and Microsoft are on par.


## Intermittent Conclusion

The message of this article is quite simple and clear: Use end-to-end encryption when data owners and system owners are not the same group of people. 
Differentiating between securing the data and securing the infrastructure is essential, otherwise the infrastructure owners become de-facto data owners from a technical perspective. So ask yourself the question: do I care if my organizations data is controlled by ourselves, or is it fine to let external organizations handle our core data?

## A deeper cryptographical intro

## The why (the cause for the new situation)

An important cause for security moving from the back to beeing pronounciated is the booming of clous computing. Most systems are now fully or partly cloud? Why? Because it is easy and csoat efficient. The downside is that the flyffy term "The Cloud" really only means "Someone elses computer". 

Think about it. Saying, "we will get a cheaper and more availabe service using the cloud" sounds much better than, "we can save cost by putting all of our core data on someone elses computer".

Using the cloud means bringin in more parties in the system, which has to be trusted. In the security business, we call that an increased attack surface. When changing the system by adding a ofr example a cloud storage service, your system can now also be attacked in that part of the system.

People should not need to fully trust the service provider and all the accopanyuing parties with their sensitive data. This is where encryption comes in to play.

![Cloud History]({{ site.url }}/images/cloudhist.png)

**Cloud History**

## The how (how is security implemented)


#### Email example for HBH

1. sender creates email and encrypts it for her email server.
2. the senders email server receives the email and decrypts it
3. the senders email server reencrypts the email for therecipients email server and send it there
4. the recipients email server receives the email, decrypts it, and then reencrypts it for the recipient.

here, the security is divided in to several steps, and the email is vulnerable everytime is decrypted and reencrypted.
it is also a system with 4 parties. The sender, the receiver, the senders email service and the receivers email service.
All of these parts needs to be trusted (we wont get into the email services thrird party services since it would be too complex, but basically, those also needs to be trusted.)

oscoap
pgp
signal
hyker

we will explain the differences in more depth later. Right now, lets revisit the email example, but use e2e

#### Email example for e2e

1. sender creates email and encrypts it for the RECIPIENT
2. the senders email server receives the email, it can see where to send it but it can not decrypt the contents
3. the senders email server sends the email to the recipients email server
4. the recipients email server receives the email, this server also can not read the contents
5. when the recipient recieves 

as you can see, no part of the system except for the intended recipient has the decryption keys. Therefore we can use the same servers as in the prior example, but we do not give them access to the data.

### rules vs crypto

this can all be condensed to rules vs crypto. in the first example, hbh, we have rules... etc
it is a question of "who has access to the keys". Many systems are fully encrypted, but the important thing is where is the control of the decryption keys.

## The why, revisited

Lets touch the cloud again. Why do we use the cloud. Because it is cheap. using the cloud could be a significant cost saving. but if you use the traditional hop-by-hop model, it is a calculated risk. 

using hop-by-hop was fine in the last decade, since you usually owned the entire system, and the users data was part of this closed system. 

so people have continued to use this model when migrating to the cloud, knowing the risks, but deeming the cost savings to be of higher priority.

when more sensitive data is now moved to the cloud, and 

## The actors (who is really part of the system)

we have stacking layers of trust. in an optimal secure system, only users are trusted. the next level of trust is the organizations itself, e.g. a company where the user works. the next level is the service provider, e.g. an online mail service. after that, we get the mail providers storage cloud. then the operating system used for storage 

we are not saying: dont trust your employer, dont trust anyone. What we are saying is "dont make the entire internet a trusted part of your system. Use all the services you want, but dont treat them like inner circle members."

## The data (what do you want/need to protect)

an important 

## The enemy (who are you protecting against)

The reason for implementing any kind of computer security is a percieved threat of data leakage, system downtime, data corruption, and other similat scenarios.
People often see the "Hacker" as the embodiment of this threat.
This is a narrow scope.

Threats are not neccecarily malicious. They can also be be errors in handling and similar situations.
Threats are also not only external. Insiders are also potential threats.
With the expansion of the enterprise into the cloud, this becomes an imortant aspect, since you are essentially making the cloud provider an insider if you dont use a proper security model.

### A parable

![Apartment keys parable]({{ site.url }}/images/apartments.png)

**Apartment Keys Parable**


## The lineup (what are my choices)

The key question you should ask yourself is: "what are my security needs". There is no such thing as "the one correcr model". Below are some questions to answer in order to point yourself in the right direcion.

 * Is the system a closed system? 
 - If your system is all within your organization (i.e. no 3rd party services, you can physically touch your servers), and all the user data is owned by the organiztion and not the users themselves, then you might be fine using traditional security (hop by hop and channel security).

 * If it is a closed system. Is every point in this system completly secure?
 - A system with hop-by-hop security is only as secure as the weakest link. If the weakest link is strong enough though, trad sec might be allright.

 * Is there any kind of sensitive data?
 - This is a highly subjective question. But a rule of thumb is: would your users be concerned if the data were to leak? If not you might also be fine using tradition security.

 * Does the user have an initiative to trust the service?
 - If the user should not be forced to trust the service provider with their data, then end-to-end is the way to go. This is especially true for cloud systems complosed of several entities, such as for example AWS, docker, etc.

In summary, if there is no sesitive data, or the data is collectively owned within a closed system with mutual trust, you are fine with traditional security.
If however, you have sendsitive data, and the user should not have to trust all of the prts of the service, you want end-to-end.

### A short system design instroduction

When describing a system, we separate the concepts of clients and servers, and connections and storage. In the most simple systems, there exists only clients and they connect directly to each other. This could be for example a walkie-talkie system. 

With more complex systems, you involve servers aswell. Think of your phone line. When you call somebody, your phone first connects to a server at the phone company, which in turns relays you to the destination phone. The server here acts a real time proxy. Both this scenario and the walkie-taklie system can be classified as synchronous systems, meaning that both endpoints are active at the same time and there is no practical delay.

The next step is letting the server act as an intermediate storage unit. Here the clients does not need to be online at the same time. Think of a chat system. In a chat system, one can send a message, turn off your smartphone, and still expect the message to be delivered. This is because the messages are cached at the intermediate server. A system of this type can be classified as asynchronous.

To wrap up, the core 2 carchteristicts in this context is:

 * Are there both clients and servers?
 * Is the communication syncronous or asynchronous.

### Traditional security

Traditional security makes a clear distinction between securing connections and devices such as servers and clients.
The focus here is to secure every part of the system. We have one type of security for the clients, one for the connection between the clients and the servers, and one for the servers. Common techniques here are: TLS, VPN, firewalls, IDS:s etc.

These tools are very good for their purpose, which is protecting the infrastucture. But as soon as we have systems where you use third party providers, you should consider using end-to-end security instead.

### End-to-end security

End-to-end data security on the other hand protects the data itself, not the infrastructure. 

### HYKER

### Signal


![E@EE Comparison]({{ site.url }}/images/comparison.png)

**E2EE Comparison**
#### whatsapp


