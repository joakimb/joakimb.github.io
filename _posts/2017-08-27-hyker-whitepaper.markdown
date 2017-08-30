---
layout: post
title: "Use Cases for a Trust Based Security Model"
date: 2017-08-04 12:00:00 +0200
---

## Introduction

Security is, and always has been, a basic requirement for quality products. It has been part of every serious system since the start of networked computing in the late 1900:s and is now part of every mans life with the moving of some parts of our lives to the internet. However, it has always been hidden under the surface. It has not been publicly advertised by neither the media or the system builders.

The situation has changed. Nowadays, security breaches is front page news on the most respected newspapers and system builders openly discuss their security model as a part of marketing.

### The HYKER mission

HYKER takes a strike at securing the data in a system, as opposed to securing the infrastrucure. HYKER separates the access control of data from the process of sending it. A HYKER message does not have an explicit addressee. There is no recipient, and nobody can decrypt it. Instead of encrypting for a known receiver, the message is tagged with metadata of how to aquire the key. It is then up to a comsuming party to request access to the decrypting key.

This makes the system highly suitable for pub-sub systems, especially when you have multiple parties involved, or the data is stored over time, or people in the sysem come and go.

This is all done using __end-to-end__ encryption, protecting the data in an ubroken chain from producer to consumer. There is no central storage of decryption keys. The important thing is that sensitive data does not touch any part of a system it does not need to.

The security of a certain part of a system is always easy. Use good encryption, install a firwewall, separate processes etc. The hard part is getting the overall security good. Being sure that there is no part of the chain that is weaker. HYKER eliminates the need to trust the ultimate security of every part of the chain by focusing on the data instead of the infrastructure. This is done through a trust based security model.


### Key insight: Encryption is easy

Want to encrypt something? Good, go get a state of the art implementation of a world class algorithm. This is available in a multitude of open source libraries. For free.

### Key insight: Key Distribution is hard

So, you have a good way of encrypting your data. How do you obtain the key? In simple systems you can use a classic PKI? But what if you don't know the reciever at send time? What if the reciever belongs to another organization that does not use a PKI? What if the keys needs to be replicated?

Key distribution and key management are genrally recognized as problems with no one-size-fits-all solution. This is a big reason for why securing a system is difficult. The owner of the keys controls tha data, and key management controls key ownership. 

### Key insight: Access Control is all about trust

parts of a system, select what you want to trust not what you need to trust. This way yoyr system design and security model is not dependent on each other, you dont need to trust a part of your system just because data flows through it.

## Secure communication

### How the data is secured

All security is based on conditionally selecting trusted parts of a system. To enforce the security - i.e. the exclusion of untrusted entities - we use the tools, rules and cryptography.
Rules are prone to human error, or malicious intents.
Cryptography on the other hand is highly resistant to these flaws.

#### Securing every part of the system (Hop-by-Hop)

Most people that have come in contact with computer security have heard about the common techniques for connection security and end node security such as TLS and firewalls.

These technologies are very good technologies, but the purpose is to secure the infrastructure, piece by piece, or as we call it in the industry, __hop-by-hop__ security.
This means that when for example sending an email, the email is encrypted in transit and in storage, but the parties delivering the mail are able to decrypt the message.
This is a weakness.
There can be vulnerable parts of the chain; or some part of the chain can be malicious.
Traditional security models like this are good for securing infrastructure that you own, it is not designed to protect data which does not explicitly belong to the system owner.

#### Securing the data at the end nodes (End-to-End)

When you do not control 100% of the infrastructure - as when using cloud services - or the data does not belong to the system - as in a chat service or a mail system, protecting the infrastructure is not enough.
Additionally, you need to secure the data itself.
If the data is encrypted at the message sender, and the decryption key is only made available to the intended recipient, you achieve what is called __end-to-end__ security (E2E).
This type of security eliminates the need to fully trust every part of the system, making for a reduced attack surface and brings data ownership back from the system to the data producer.

There are many technologies available for E2E, e.g. [HYKER](https://hyker.io), [PGP](http://openpgp.org), [Signal](https://whispersystems.org) and [OSCoAP](https://tools.ietf.org/html/draft-selander-ace-object-security-05). 
They all achieve fully secure E2E, but with important differences making them suited for different purposes.

## Data Life Cycle Security

### What is trust?

Trust is related to abilities. What abilities do I trust a certain party with?

In a secure system, trusted parties are those who have access to data, i.e. they are able to obtain decryption keys. This entails that in a __hop-by-hop__ system, all hops are trusted. In __end-to-end__ systems however, only the end points are trusted. This means that we should opt for __end-to-end__ based systems if there is sensitive data, or data not owned by the system edministrators, involved.

### Data life cycle

Data is data, unrelated to what system it flows through. Therefore we need to focus on the full data lifecycle, from production to consumption, over time, using any system. This is what HYKER does.
The HYKER innovation consists of the separation of encryption and access control. This is the enabler for trust based security, where you can select trusted parts of a system based on the data, not on how the infrastructure is designed. It also enables voulontairy delegation of control and retroactive access.

### Retroactive access

### Access Control Delegation

## Example Use Cases

### Autonomous Vehicles

Autonomous vehicles are going to communicate alot. The most basic form of communication is car-to-car, where vehicles share things like current speed and direction, known obstacles, etc. with nearby vehicles. If the vechicle is part of a fleet, it will also have to be able to communicate things like current position and payload to a traffic management system.

As this information can be sensitive it is important for the vehicle to be able to set rules for how the data propagates and with whom it is shared. That is, detected obstacles can be public data, position and heading made available to nearby vehicles, and routing plans are shared only with associated vehicles (e.g. within the same fleet). The groups formed will be of varying dynamicity; the nearby-vehicles-group will be highly dynamic while the fleet remains fairly consistent. The forming of these groups, and the maintaining of a healthy state over time by including and excluding members, is a central problem for autonomous systems as agents need to trust the consistency of such groups in order to trust the data.

This is a basic example of the security needed in a self-organizing system where agents can reach consensus in a distributed way. It also suggests how to restrict propagation of sensitive information without human administration.

In scenario like this, with a shared infrastructure that is constantly changing, security will be unobtainable using traditional ifrastructure-based methods. Instead we will need a trust based model like HYKER.

### Predictive maintenance

In the simplest form of predictive maintenance, where all of the infrastucture is owned by the same company that conducts the maintenance and the data is not sensitive, there is no place for HYKER. Traditional security will do just fine.

But as soon as we start to enter more evolved scenarios, such as predicive maintenance as a service, military operations, or critical infrastructure, we start having to deal with the questions posed above. What part of the system is exposed to third parties, does the system use any kinnd of cloud service, who is collaboratively using data, and so on. A single RPM value from an engine is probably not sensitive at all, but aggregated data from continuous predictive maintenance can be powerful information mapping the properties, systems and current state of an organization. An example of that predictive maintenance data is considered sensitive is available in a recent [report from ICF](https://www.icf.com/-/media/files/icf/white-papers/2017/aviation_aerospace_ahm_aircraft_big_data_analytics_health_monitoring_wp.pdf), where the question of data ownership is explored and deemed central. This is where a trust based security model can be of help.

Take a critical infrastructure example: a powerplant. The powerplant uses one firm to conduct the automatic infrastructure surveillance, and various contractors to repair and replace parts. This means that we
have many

An operation like this world be difficult to propely secure with traditional techniques, since you would have to put a lot of unneccecary trust in the different parties. With a trust base security model like HYKER however, it would be alot easier since you can selectively pick what parties should be trusted with what data on a very granular level.

In a recent [report](https://www.foi.se/en/pressroom/news/news-archive/2017-08-29-risks-with-online-societal-services.html) from the swedish Defence Research Agency, it is concluded that the current computer security at swedish critical infrastructure, like powerplants etc., is serverely substandard. The report suggests that cutting the cord, i.e. disconnecting all infrastructure from the internet, is a viable solution.

We dont belive in the proposed solution which would bring us back to the 90:s. Instead we belive in a proper security model.

### Telco messaging
Data flows through the network of a Telco, but it is not in a single line. The systems are complex networks of communication links and relaying nodes, routers, servers, data storages etc. Many from different 3rd party suppliers. Each link and each node is usually protected with perimeter security models, like encrypted tunnels, SSL/TLS protocols, etc and the nodes have their access control, locked down ports etc. This is network security, or hop-by-hop security. Data is encrypted in transit over a link, but decrypted into clear text in the node and re-encrypted for the next link.

This means that the Telco, and the Telco customers, must trust each and every node, the staff that has access to the node and the developer and supplier of this node.

Many customers have high requirements on security and are uncomfortable or even legally required not to trust 3rd party networks or cloud storages. Through GDPR et al they are required to maintain full control over their and their clients data, disregarding who they have contracted to store or process that data. 

HYKER provides a technology where data is protected in an unbroken chain from the data producer to the data consumer, in transit, at rest and over time, independent of network technology. A form of end-to-end encryption technology but with managed trust and data access control. 

Instead of having trust distributed and delegated as a result of the different network and cloud suppliers models, the customer can take full control over the system and manage trust to best suit the security needs and logics of their application.

Extended offering
A Telco that sells connectivity and capacity to transmit sensitive data for a client can extend their offerings with HYKER end-to-end encryption and managed trust. This can be done in different ways depending on the business models of the Telco. 

#### Some general examples:
 * You can different on security level, with a premium communication service that is fully end-to-end protected in an unbroken chain, even if you share network resources with other Telcos. You can be the most secure communication link that the customer doesn’t have to trust from a data access point of view.
 * You can provide service and application developer partners with a new security tool that protects their data through the full data lifecycle.
 * You can offer storage or cloud services to customers with sensitive information. You store their encrypted data, but have no access to keys and thus no access to the data itself.
 * If the Telco provide message broker services like MQTT for IoT, HYKER uniquely adds an end-to-end encryption layer that fully follows the traffic patterns of one-to-many publish-subscribe communication, enabling a data producer to encrypt the data without knowing the recipient(s).

### Logistgics
[TODO] (fredrik skriver)

### Insurance

For an insurance, information about incidents is of importance. This can in some cases pose a privacy intrusion. Lets explore a scenario where HYKER could ease the situation.

The scenario revolves around a car rental service. This car rental service consists of a fleet owner and a bunch of cars. The biggest asset of the rental service is, of course, the cars. Lately there has been a number of car thefts. In order to deal with the car theft problem, the owner wants to fit the cars with GPS trackers, so that the insurance company can see what has happened. The customers position being known by the car rental company and the insurance company all the time would be a privacy infringement and would hurt the public image of the company.

The solution can be a trust based security model. In day-to-day use, the car broadcasts an encrypted position that only the driver can read. If a car is stolen, the decryption key can be shared with the insurance company to prove the cusomers innosence. The decision of when to allow the rental company access to data needs to be in the hand of the customer, otherwise he or she can never know if the position is private. This is a clear scenario where trust is the main security concern. HYKER easily lets you build systems with a trust model where you can add parties retroactively.


### Surveillance
[TODO]

### Home Care
[TODO]

### Smart City
[TODO]

### General IoT
[TODO]

### Manufacturing
[TODO]



## A deeper cryptographical intro

## The why (the cause for the new situation)

An important cause for security moving from the back to beeing pronounciated is the booming of cloud computing. Most systems are now fully or partly cloud based, which is great for efficiency and feature reasons. The downside is that the term "The Cloud" really only means "Someone elses computer". Using the cloud means bringing in more parties in the system, which have to be trusted. In the security business, we call that an increased attack surface. When changing a system by adding a cloud component, your system can now also be attacked in that part of the system, which you might not be in control over.

People should not need to fully trust the service provider and all the accompanying parties with their sensitive data. This is where encryption comes in to play.

In the picture below we provide a security perspective on cloud services. In the first pane, the pre-cloud era, organizations used perimeter security. They made sure that all critical infrastructure was protected by firewalls IDS:s etc.

The second pane is an illustration of when organizations strarted using cloud services, effectively sharing control over their services and data. This was a calculated risk. Knowing that they risked losing data, they gained efficiency and cost savings.

A trust based security model is is illustrated in the last pane. Here, you still use the cloud service, but you don't consider them trusted. I.e. you keep control within you own organization by only letting the cloud service act on encrypting data. The keys never leave the organization.

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
With HYKER you can select your own trust model, without having to adapt it to the topology of your system.


![Trust Models]({{ site.url }}/images/trust-model.jpg)

### Signal


![E2EE Comparison]({{ site.url }}/images/comparison.png)

**E2EE Comparison**

## Intermittent Conclusion

The message of this article is quite simple and clear: Use end-to-end encryption when data owners and system owners are not the same group of people. 
Differentiating between securing the data and securing the infrastructure is essential, otherwise the infrastructure owners become de-facto data owners from a technical perspective. So ask yourself the question: do I care if my organizations data is controlled by ourselves, or is it fine to let external organizations handle our core data?



