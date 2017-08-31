---
layout: post
title: "Use Cases for a Trust Based Security Model"
pdf: true
date: 2017-08-04 12:00:00 +0200
---

## Introduction

Security is, and always has been, a basic requirement for quality products. It has been part of every serious system since the start of networked computing in late 1900:s and is now part of every mans life with the moving of some parts of our lives to the internet. However, it has always been hidden under the surface. It has not been publicly advertised by neither the media nor the system builders.

The situation has changed. Nowadays, security breaches are front page news on the most respected newspapers and system builders openly discuss their security model as a part of marketing.

### The HYKER mission

HYKER takes a strike at securing the data in a system, as opposed to securing the infrastructure. HYKER separates the access control of data from the process of sending it. A message protected by HYKER does not have an explicit addressee. There is no recipient, and nobody can decrypt it. Instead of encrypting for a known receiver, the message is tagged with metadata of how to acquire the key. It is then up to a consuming party to request access to the decrypting key.

This makes the system highly suitable for pub-sub systems, especially when you have multiple parties involved, or the data is stored over time, or people in the system come and go.

This is all done using __end-to-end__ encryption, protecting the data in an unbroken chain from producer to consumer. There is no central storage of decryption keys. The important thing is that sensitive data does not touch any part of a system it does not need to.

The security of a certain part of a system is always easy. Use good encryption, install a firewall, separate processes etc. The hard part is getting the overall security good. Being sure that there is no part of the chain that is weaker. HYKER eliminates the need to trust the ultimate security of every part of the chain by focusing on the data instead of the infrastructure. This is done through a trust based security model.


### Key insight: Encryption is easy

Want to encrypt something? Good, go get a state of the art implementation of a world class algorithm. This is available in a multitude of open source libraries. For free.

### Key insight: Key Distribution is hard

So, you have a good way of encrypting your data. How do you obtain the key? In simple systems, you can use a classic PKI. But what if you don't know the receiver at send time? What if the receiver belongs to another organization that does not use a PKI? What if the keys need to be replicated?

Key distribution and key management are generally recognized as problems with no one-size-fits-all solution. This is a big reason for why securing a system is difficult. The owner of the keys controls the data, and key management controls key ownership. 

### Key insight: Access Control is all about trust

Since access control governs access to data, it is very closely related to trust. Access control separates authorized entities from outsiders. Therefore, the part of the system that runs the access control is all powerful, if it is compromised or erroneously configured it can give access to anything it wants. Even itself.

In traditional __hop-by-hop__ systems, access control is based on a set of rules. There are various parts of a system which holds data, and they give access based on the given rule set. This rule set can be circumvented, either by a configuration error or mischevious behavior from external attackers or insiders. In a system like this, you are forced to trust the data holders since they have full power.

If the access control is instead based on __end-to-end__ cryptography, you do not need to trust the data holders with access control. You can strip them of all power and handle access control through cryptography instead of rules.

HYKER believes in a security design where you can select what you want to trust not what you need to trust. This way your system design and security model is not dependent on each other, you don't need to trust a part of your system just because data flows through it.

## Secure communication

### How the data is secured

All security is based on conditionally selecting trusted parts of a system to handle your data. To enforce the security - i.e. the exclusion of untrusted entities - we use the tools, rules, and cryptography.
Rules are prone to human error, or malicious intents.
Cryptography, on the other hand, is highly resistant to these flaws.

#### Securing every part of the system (Hop-by-Hop)

Most people that have come in contact with computer security have heard about the common techniques for connection security and end node security such as TLS and firewalls.

These technologies are very good technologies, but the purpose is to secure the infrastructure, piece by piece, or as we call it in the industry, __hop-by-hop__ security.
This means that when for example sending an email, the email is encrypted in transit and in storage, but the parties delivering the email are able to decrypt the message.
This is a weakness.
There can be vulnerable parts of the chain, or some part of the chain can be malicious.
Traditional security models like this are good for securing infrastructure that you own, it is not designed to protect data which does not explicitly belong to the system owner.

#### Securing the data at the end nodes (End-to-End)

When you do not control 100% of the infrastructure - as when using cloud services - or the data does not belong to the system - as in a chat service or an email system, protecting the infrastructure is not enough.
Additionally, you need to secure the data itself.
If the data is encrypted at the message sender, and the decryption key is only made available to the intended recipient, you achieve what is called __end-to-end__ security (E2E).
This type of security eliminates the need to fully trust every part of the system, making for a reduced attack surface and brings data ownership back from the system to the data producer.

There are many technologies available for E2E, e.g. [HYKER](https://hyker.io), [PGP](http://openpgp.org), [Signal](https://whispersystems.org) and [OSCoAP](https://tools.ietf.org/html/draft-selander-ace-object-security-05). 
They all achieve fully secure E2E, but with important differences making them suited for different purposes.

### A parallel

![Apartment keys parable]({{ site.url }}/images/apartments.png)

## Data Life Cycle Security

### What is trust?

Trust is related to abilities. What abilities do I trust a certain party with?

In a secure system, trusted parties are those who have access to data, i.e. they are able to obtain decryption keys. This entails that in a __hop-by-hop__ system, all hops are trusted. In __end-to-end__ systems, however, only the end points are trusted. This means that we should opt for __end-to-end__ based systems if there is sensitive data, or data not owned by the system administrators, involved.

### Data life cycle

Data is data, unrelated to what system it flows through. Therefore we need to focus on the full data lifecycle, from production to consumption, over time, using any system. This is what HYKER does.
The HYKER innovation consists of the separation of encryption and access control. This is the enabler for trust based security, where you can select trusted parts of a system based on the data, not on how the infrastructure is designed. It also enables voluntary delegation of control and retroactive access.

### Retroactive access

Think of a document sharing service, like Dropbox. The amount of data stored in the cloud can grow very large over time. If you want to add people who are allowed to read the documents, how do you do that? How can you keep control over the data without having to re-send it?

When designing a system based on hop-by-hop security, you do not need to worry about knowing all receivers beforehand. The central server can take care of adding additional receivers. But in these systems, you give up the control over your data. The central server being able to add recipients is enabled through letting it control your data, demanding total trust in it.

To retake control, you can introduce end-to-end security. But this introduces another problem. In a standard end-to-end security system, the intermediate parties such as a storage server, have no access to the data. This means that the end nodes have no control, resulting in no possibility to add recipients without resending the data. 

HYKER solves this through a concept we call __retroactive access control__. This concept brings the best of both worlds you can have full control over who gets access to your data, but you don't need to resend it. This done through separating the key sharing from the data sharing.

### Access Control Delegation

It is easy to wind up in a binary world when reasoning about end-to-end vs hop-by-hop. Especially when considering access control. In systems that are fully hop-by-hop, the central service is in full control. In systems that are fully end-to-end, the end nodes are in full control.

We have talked about why you might not want to give full control to a central server. There are also cases where you don't want full control in the end nodes either. An example could be an enterprise communication platform where a decentralized security model with elevated control for middle managers could be desired.

Take a look at the picture below. This picture illustrates the differences between centralized, decentralized and distributed systems.

![Trust Models]({{ site.url }}/images/trust-model.jpg)

We solve the binary situation by using delegated access control, making a combination of distributed, decentralized and centralized trust models possible.

Delegated access control is a concept where an end-node can voluntarily delegate permission of key sharing to any other node. If an employee delegates the decryption key responsibility to the nearest manager, that manager gains the possibility to let other employees access the data. Another example is an IoT sensor which should not have any say in who can access the information. That sensor can delegate the access control to the sensor owner or a group of owners.

## Example Use Cases

### Predictive maintenance

In the simplest form of predictive maintenance, where all of the infrastructure is owned by the same company that conducts the maintenance and the data is not sensitive, there is no place for HYKER. Traditional security will do just fine.

But as soon as we start to enter more evolved scenarios, such as predictive maintenance as a service, military operations, or critical infrastructure, we start having to deal with the questions posed above. What part of the system is exposed to third parties, does the system use any kind of cloud service, who is collaboratively using data, and so on. A single RPM value from an engine is probably not sensitive at all, but aggregated data from continuous predictive maintenance can be powerful information mapping the properties, systems and the current state of an organization. An example of that predictive maintenance data is considered sensitive is available in a recent [report from ICF](https://www.icf.com/-/media/files/icf/white-papers/2017/aviation_aerospace_ahm_aircraft_big_data_analytics_health_monitoring_wp.pdf), where the question of data ownership is explored and deemed central. This is where a trust based security model can be of help.

Take a critical infrastructure example: a power plant. The power plant uses one firm to conduct the automatic infrastructure surveillance, and various contractors to repair and replace parts. This means that we have many different parties acting in a shared system with different data access rights.

An operation like this would be difficult to properly secure with traditional techniques since you would have to put a lot of unnecessary trust in the different parties. With a trust based security model like HYKER however, it would be easier since you can selectively pick what parties should be trusted with what data on a very granular level.

In a recent [report](https://www.foi.se/en/pressroom/news/news-archive/2017-08-29-risks-with-online-societal-services.html) from the Swedish Defence Research Agency, it is concluded that the current computer security at Swedish critical infrastructure, like power plants etc., is severely substandard. The report suggests that cutting the cord, i.e. disconnecting all infrastructure from the internet, is a viable solution.

We don't believe in the proposed solution which would bring us back to the 90:s. Instead, we believe in a proper security model.

### Telco messaging
Telco systems are complex networks of communication links, relaying nodes, routers, servers, data storages etc. Many of these nodes are from different 3rd party suppliers. Each link and each node is usually protected with perimeter security models, like encrypted tunnels, SSL/TLS protocols, etc. This is network security, or hop-by-hop security. Data is encrypted in transit over a link, but decrypted into clear text in the node and re-encrypted for the next link.

This means that the Telco, and the Telco customers, must trust each and every node, the staff that has access to the node and the developer and supplier of this node.

Many customers have high requirements on security and are uncomfortable or even legally required not to trust 3rd party networks or cloud storages. Through GDPR et al they are required to maintain full control over their and their clients data, disregarding who they have contracted to store or process that data. 

HYKER provides a technology where data is protected in an unbroken chain from the data producer to the data consumer, in transit, at rest and over time, independent of network technology. A form of end-to-end encryption technology but with managed trust and data access control. 

Instead of having trust distributed and delegated as a result of the different network and cloud suppliers models, the customer can take full control over the system and manage trust to best suit the security needs and logics of their application.

#### Extended offering
A Telco that sells connectivity and capacity to transmit sensitive data for a client can extend their offerings with HYKER end-to-end encryption and managed trust. This can be done in different ways depending on the business models of the Telco. 

##### Some general examples:

 * You can differentiate offers on the security level, with a premium communication service that is fully end-to-end protected, even when you share network resources with other Telcos. You can be the most secure communication link that the customer doesnâ€™t have to trust from a data access point of view.
 * You can provide service and application developer partners with a new security tool that protects their data through the full data lifecycle.
 * You can offer storage or cloud services to customers with sensitive information. You store their encrypted data, but have no access to keys and thus no access to the data itself.
 * If the Telco provides message broker services, like MQTT servers for IoT, HYKER uniquely adds an end-to-end encryption layer that fully follows the traffic patterns of one-to-many publish-subscribe communication, enabling a data producer to encrypt the data without knowing the recipient(s).

### Logistics

Logistics generates and consumes an abundance of data, from many sources and some of it sensitive or have other business value. Big Data is a huge trend that will have a major impact on logistics business.

Data is the key success factor of big data and as such, it is of great value. Logistics systems have many endpoints, for instance, tracking devices on trucks, sensors and tracking devices on goods and containers, driver devices (cell phones, tablets) with work orders and reports, etc. There are also central endpoints like dispatch management terminals, analytics software and so on.

Since logistics revolves around a hot pot of different systems and actors, data ownership can be a real challenge using traditional security. The important thing is to have control over what data should be accessed by whom and when. Should a customer be able to follow their shipment in real-time? Should trucks be able to communicate directly with other trucks? With some trucks but not all trucks? Organized in groups that change constantly?

Instead of having trust distributed and delegated as a result of the different network and technology suppliers models, the customer can use HYKER to take full control over the system and manage trust to best suit the security needs and logics of their application.

### Insurance

For an insurance, information about incidents is of importance. This can in some cases pose a privacy intrusion. Let's explore a scenario where HYKER could ease the situation.

The scenario revolves around a car rental service. This car rental service consists of a fleet owner and a bunch of cars. The biggest asset of the rental service is, of course, the cars. Lately, there has been a number of car thefts. In order to deal with the car theft problem, the owner wants to fit the cars with GPS trackers, so that the insurance company can see what has happened. The customer's position being known by the car rental company and the insurance company all the time would be a privacy infringement and would hurt the public image of the company.

The solution can be a trust based security model. In day-to-day use, the car broadcasts an encrypted position that only the driver can read. If a car is stolen, the decryption key can be shared with the insurance company to prove the customers innocence. The decision of when to allow the rental company access to data needs to be in the hand of the customer, otherwise, he or she can never know if the position is private. This is a clear scenario where trust is the main security concern. HYKER easily lets you build systems with a trust model where you can add parties retroactively.

### Autonomous Vehicles

Autonomous vehicles are going to communicate constantly. The most basic form of communication is car-to-car, where vehicles share things like current speed and direction, known obstacles, etc. with nearby vehicles. If the vehicle is part of a fleet, it will also have to be able to communicate things like current position and payload to a traffic management system.

As this information can be sensitive it is important for the vehicle to be able to set rules for how the data propagates and with whom it is shared. That is, detected obstacles can be public data, position, and heading made available to nearby vehicles, and routing plans are shared only with associated vehicles (e.g. within the same fleet). The groups formed will be of varying dynamicity; the nearby-vehicles-group will be highly dynamic while the fleet remains fairly consistent. The forming of these groups, and the maintaining of a healthy state over time by including and excluding members is a central problem for autonomous systems as agents need to trust the consistency of such groups in order to trust the data.

This is a basic example of the security needed in a self-organizing system where agents can reach consensus in a distributed way. It also suggests how to restrict propagation of sensitive information without human administration.

In a scenario like this, with a shared infrastructure that is constantly changing, security will be unobtainable using traditional infrastructure-based methods. Instead, we will need a trust based model like HYKER.

<!---
## A deeper cryptographical intro

### The why (the cause for the new situation)

An important cause for security moving from the back to beeing pronounciated is the booming of cloud computing. Most systems are now fully or partly cloud based, which is great for efficiency and feature reasons. The downside is that the term "The Cloud" really only means "Someone elses computer". Using the cloud means bringing in more parties in the system, which have to be trusted. In the security business, we call that an increased attack surface. When changing a system by adding a cloud component, your system can now also be attacked in that part of the system, which you might not be in control over.

People should not need to fully trust the service provider and all the accompanying parties with their sensitive data. This is where encryption comes in to play.

In the picture below we provide a security perspective on cloud services. In the first pane, the pre-cloud era, organizations used perimeter security. They made sure that all critical infrastructure was protected by firewalls IDS:s etc.

The second pane is an illustration of when organizations strarted using cloud services, effectively sharing control over their services and data. This was a calculated risk. Knowing that they risked losing data, they gained efficiency and cost savings.

A trust based security model is is illustrated in the last pane. Here, you still use the cloud service, but you don't consider them trusted. I.e. you keep control within you own organization by only letting the cloud service act on encrypting data. The keys never leave the organization.

![Cloud History]({{ site.url }}/images/cloudhist.png)

**Cloud History**

Using a security model based on trust can allow an organization to use the cloud with all the benefits, without sacrificing control.

### The actors (who is really part of the system)

A system always has many different views of trust. The users have their view of what they trust, the system amintainers their, and so on. This means stat we have stacking layers of trust. In an optimally secure system, end users are the only trusted entities. The next level of trust can be the organizations itself, e.g. a company where the user works. After that it can be the the service provider, e.g. an online email service. After that, we get the email providers storage cloud, then the operating system used for storage and so on.

To be able to differentiate between the different layers of trust, a trust based model is needed.

### A parable

![Apartment keys parable]({{ site.url }}/images/apartments.png)

**Apartment Keys Parable**

## The lineup (what are my choices)

The key question you should ask yourself when choosing a security model is: "what are my security needs". There is no such thing as "the one correct model". Below are some questions to answer in order to point yourself in the right direcion.

 * Is the system a closed system? 
 - If your system is all within your organization (i.e. no 3rd party services, you can physically touch your servers), and all the user data is owned by the organiztion and not the users themselves, then you might be fine using traditional security (hop by hop and channel security).

 * If it is a closed system. Is every point in this system completly secure?
 - A system with hop-by-hop security is only as secure as the weakest link. If the weakest link is strong enough though, trad sec might be allright.

 * Is there any kind of sensitive data?
 - This is a highly subjective question. But a rule of thumb is: would your users be concerned if the data were to leak? If not you might also be fine using tradition security.

 * Does the user have an initiative to trust the service?
 - If the user should not be forced to trust the service provider with their data, then end-to-end is the way to go. This is especially true for cloud systems complosed of several entities, such as for example AWS, docker, etc.

In summary, if there is no sesitive data, or the data is collectively owned within a closed system with mutual trust, you are fine with traditional security. If however, you have sendsitive data, and the user should not have to trust all of the parts of the service, you want end-to-end.

## Appendix

The traditional internet landscape consists of cables and big central servers. Clients pushes and pulls data to and from servers.

The current internet landscape is moving away from that reality, and has been for some time. A large portion of devices, e.g. phones or IoT-devices, communicate over wireless channels. Centralized systems are becoming more and more distributed and devices often communicate directly without a central point.

The changed communication landscape needs a new approach for communication security, and HYKER solves many of the inherent security problems.

When describing a system, we separate the concepts of clients and servers, and connections and storage. In the most simple systems, there exists only clients and they connect directly to each other. This could be for example a walkie-talkie system. 

With more complex systems, you involve servers aswell. Think of your phone line. When you call somebody, your phone first connects to a server at the phone company, which in turns relays you to the destination phone. The server here acts a real time proxy. Both this scenario and the walkie-taklie system can be classified as synchronous systems, meaning that both endpoints are active at the same time and there is no practical delay.

The next step is letting the server act as an intermediate storage unit. Here the clients does not need to be online at the same time. Think of a chat system. In a chat system, one can send a message, turn off your smartphone, and still expect the message to be delivered. This is because the messages are cached at the intermediate server. A system of this type can be classified as asynchronous.

To wrap up, the core 2 carchteristicts in this context is:

 * Are there both clients and servers?
 * Is the communication syncronous or asynchronous.

### Synchronicity and Asynchronicity

At the core of the changing landscape is a transfer from synchronous to asynchronous communications patterns. The shift is inherent to how many applications function.

We are not saying that synchronous communication is going away, because it certainly is not. However the portion of traffic occurring over asynchronous protocols is growing, and the security needs to be tended to.

#### Synchronous communication

Synchronous communication is information exchange between parties online at the same time. It is analogous to a phone call:

* The communication is initialized by one party.
* The information exchange start after the second party accepts the offer.
* No party can continue without the other.
* The exchange is terminated simultaneously.

This is very handy when you have a direct communication line to the other party and require some form of interaction, i.e. a response or a confirmation. There is no need for an intermediate routing node, e.g. a server, and you are always aware of whom you are communicating with.

##### Typical examples:

TCP, HTTP.

#### Asynchronous communication

Asynchronous communication on the other hand does not require both parties to be online simultaneously. It is more like sending a letter:

* The communication starts directly, it does not require interaction from a recipient.
* A sender can continue sending without interaction from another party.
* The exchange is not terminated since it is not an active connection.

This is handy when you want to send a message somewhere and do not care for an intermediate response. E.g. an email exchange does not require immediate responses, responses can be sent days afterwards.

Further, asynchronous communication will require an intermediate to store the message while waiting for the recipient to come online. This is analogous to the postal service when delivering a letter. Using an intermediate brings other advantages; we do not have to decide whom the recipient is at send time. Instead this can be decided at a later time by the intermediate. Such an intermediate is often called a broker.

##### Typical examples:

email, MQTT, AMQP, SMS.

### Request-Response and Publish-Subscribe

Another way to categorize communication is in Request-Response and Publish-Subscribe.

#### Request-Response

Request-response is a communication pattern for communication between 2 parties. Every message sent from one party requires a response from the other. A typical example is HTTP, where a client sends a request for a web page and the server responds with the page or an error code. Client-server scenarios such as this are often implemented using request-response patterns.

#### Publish-Subscribe

Publish-subscribe systems are of another nature. Here we have a single sender publishing a message to several recipients. The key word here is **publish**, i.e. the message is not for a single recipient, but for a group of recipients.

When publishing on a network or through a caching intermediate such as a broker, we do not have to know all the recipients. It can be endpoint in a specific block, subscribers to a certain new channel, or something else.

An example of a system using a publish-subscribe messaging pattern is a thermometer broadcasting its current value to whoever is interested.

This broadcasting can take place over synchronous channels such as ethernet, or asynchronous channels such as MQTT. Using an asynchronous channel enables a time perspective where the recipient can fetch historic messages and therefore does not have to be constantly online in order to not miss out.

## Security patterns

The suitable security model to use is dependent on how the system communicates. In this section we describe relevant characteristics of communications security.

### Channel Security and Object Security

#### Channel Security

Channel security is a means of achieving a secure channel between 2 endpoints. The channel is negotiated and managed by a protocol at the data, network or transport level in the protocol stack. The channel handles the data agnostically; it does not know anything about the payload.

This security model is suitable for securing a synchronous connection between 2 parties, since it requires interaction between the parties.

#### Object Security

Object security is a way of achieving secure communication with no need for a secure channel. Instead of relying on a communication protocol lower in the stack to handle the encryption, the application that created the message will handle encryption and decryption of its own communication.

The difference between channel security and object security may seem subtle, the key difference is that in object security an application handles its own secure communication. This has the effect that when using object security, an application does not need to rely on a secure channel. Rather, it can pick and choose by itself what to protect and how to protect it. This is a very important aspect, as we shall see.

Since object security encrypts the payload of a message, it is sometimes referred to as **Payload Encryption**.

### End-to-end and Hop-by-hop

When communicating through an intermediate, we can either terminate the security in the intermediate or let the message stay encrypted all the way through. This is calles using end-to-end or hop-by-hop security.  
Hop-by-hop security

#### Hop-by-hop Security

Hop-by-hop security, visualized in Figure 1, is to terminate the channel security session in the caching node, effectively dividing the security in two parts. One part from the sender to the caching node and one part from the caching node to the receiver.  
The data is force to be decrypted when is enters the intermediate node.

![Hop-by-hop security](./hbh_900.png)

**Figure 1**

#### End-to-end Security

End-to-end security, visualised in Figure 2, is to not terminate the session at the proxy but instead keep security through the proxy. This thwarts the possibility for the proxy to attack the session in a meaningful way since it prevents it from reading the data or changing it without detection.

![End-to-end security](./e2e_900.png)

**Figure 2**

### The common security situation today

Today, many services are secured by channel security. This is perfectly fine and actually preferable for the many synchronous request-response applications out there, e.g. web pages.

The problem is that the same security tools have been used to secure asynchronous publish-subscribe systems as well, such as MQTT.

#### The clash between End-to-end security and channel security

When securing asynchronous communication systems using an intermediate proxy, a common approach has been to use hop-by-hop channel security.

Hop-by-hop security is undesirable for many reasons. Among other reasons, hop-by-hop security is undesirable because:

* Privacy is unattainable if an intermediate proxy can decrypt or alter data
* Giving an intermediate proxy access will increase the attack surface
* Giving an intermediate proxy access will create a single point of failure for the security
* Control over data is not in the hand of the sender and receiver if an intermediate proxy has access
* In a multi-party system, where many organizations collaborate, a centralized intermediate with communication access renders data channel separation unsolvable

#### Advantages of transitioning to Object Security

Since asynchronous communication requires intermediates, we need Object Security in order to achieve end-to-end security. With object security, only the payload is encrypted, the session data stays untouched. Thus, an encrypted payload can be sent to an intermediate proxy which has no ability do decrypt or alter the data. The payload can then be fetched by recipients with decryption abilities. This way, the data is encrypted by the same key through the entire chain, both in transit to the intermediate, at rest while cached at the intermediate, and finally in transit to a recipient.

The most popular IoT protocols today are asynchronous with intermediate proxies.

Examples of publish-subscribe ones are:

* MQTT
* AMQP

Request-response ones are:

* CoAP

In conclusion, object security enables end-to-end security without preventing proxy operations.

### The HYKER role
With HYKER you can select your own trust model, without having to adapt it to the topology of your system. This is enabled through the combination of end-to-end security, retroactive access control and delegation of control.

## Conclusion

The message of this article is quite simple and clear: Use end-to-end encryption when data owners and system owners are not the same group of people. 
Differentiating between securing the data and securing the infrastructure is essential, otherwise the infrastructure owners become de-facto data owners from a technical perspective. So ask yourself the question: do I care if my organizations data is controlled by ourselves, or is it fine to let external organizations handle our core data?


--->
