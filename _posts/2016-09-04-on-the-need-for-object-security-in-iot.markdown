---
layout: post
title: "On the need for object security in IoT"
date: 2016-09-04 12:00:00 +0200
---

_The Internet of Things (IoT) is coming. With it comes new security challenges from the constrained nature of IoT devices. This blog post discusses how information security is affected by the constrained and asynchronus nature of IoT. Much of the text is an adaptaion from the master thesis [Compact Object Security for the Internet of Things](http://lup.lub.lu.se/student-papers/record/8887542) written by me and [Martin Gunnarsson](mailto:martin.gunnarsson.782@student.lu.se) in 2016._

### Introduction
Let‘s start by talking about what IoT really means. IoT is a wide term used to describe anything from industrial control systems to smart homes. IoT devices are thought of ranging from Raspberry PIs to  NFC chips. Everybody has their own view. Much of the challenges associated with IoT devices springs from the nature of [_constrained nodes_](https://tools.ietf.org/html/rfc7228#section-2.1). These nodes, or devices, are constrained in the sense that they have low processing power, a small amount of RAM and often operate on battery power. 


### Terminology

##### Channel Security
[_Channel security_](https://tools.ietf.org/html/rfc3552#section-4.7) is a term used in communications security which describes a secure channel used by an application to transmit data. The channel is negotiated and managed by a protocol at the data, network or transport level in the protocol stack. The channel handles the data agnostically; it does not know anything about the payload.

##### Object Security
[_Object security_](https://tools.ietf.org/html/rfc3552#section-4.7) is a term used in communications security describing secure communication with no need for a secure channel. Instead of relying on a communication protocol lower in the stack to handle the encryption, the application that created the message will handle encryption and decryption of its own communication. 

The difference between channel security and object security may seem subtle, the key difference is that in object security an application handles its own secure communication. This has the effect that when using object security, an application does not need to rely on a secure channel. Rather, it can pick and choose by itself what to protect and how to protect it. This is a very important aspect, as we shall see.


### Characteristics of IoT networks
The characteristics of constrained devices calls for a different set of solutions than the ones commonly used for non-constrained devices. The info-sec community needs to explore how to provide information security on constrained devices communicating over insecure channels, i.e, how can message integrity, secrecy, replay protection, message freshness guarantee, and sequence ordering be provided for the Internet of Things.

There are some properties of constrained networks which have significant impact on how communication works compared to how it works on traditional networks.

Firstly, communicating parties operating on battery power will naturally want to save battery. Therefore the network part of a constrained device will be turned off for as much time as possible. This means that the normal case will be asynchronous communication and because of this, caching nodes are the norm in IoT. 

Second, when sending data on the network, you want to send as little data as possible. This is of course always a good thing but it is extra important for constrained nodes; power consumption from network hardware is very high compared to cryptographic operations. Another aspect of minimizing transmitted data is that large data packages will require larger buffers on the recipient node, something that might not be available on constrained nodes. 

Sending large packages also increases the retransmission amount. If a fragment of a large message is dropped by the network, or there is an error somewhere in a packet, all of the message might need to be retransmitted instead of just a small part. Packet fragmentation can be a considerable source for network overhead.


### The situation, and what we should do about it.

#### Traditional methods
Secure communication for constrained nodes can certainly be achieved using traditional methods such as channel security. A popular channel security protocol for constrained devices is [DTLS](https://tools.ietf.org/html/rfc6347). 

The security in these traditional protocols protects the channel, not the data itself, hence the name. Since many devices operate on battery power it is important to use as little resources as possible, both in terms of power consumption and memory/CPU usage. The consequences of this is very often seen in devices designed so that they sleep for as much time as possible. 

The core problems with security in IoT is that Application traffic is asynchronous, which makes caching a requirement for a well functioning network. To achieve this, caching proxies are often used.

#### Consequences of using traditional methods
Herein lies the core problem of this article. If a proxy is to be used for caching data, and channel security is used, the security can follow two patterns. Hop-by-hop or end-to-end security.


##### Hop-by-hop security
The first pattern, _hop-by-hop_ security, visualized in Figure 1,is to terminate the channel security session in the caching node, effectively dividing the security in two parts. One part from the sender to the caching node and one part from the caching node to the receiver. 

![Hop-by-hop security]({{ site.url }}/images/hbh_900.png)

**Figure 1**

Doing this forces the data to be decrypted at the caching node; the caching node needs to re-encrypt the plaintext for the receiver. The data integrity and confidentiality will therefore not be end-to-end between the client and server, but hop-by-hop from client to proxy and from proxy to server. 

Hop-by-hop security can only be relied on if all partners are trusted. This is not a good assumption for a secure and robust system. The possibility of malicious nodes opens up for both passive eavesdropping attacks and active attacks such as man-in-the-middle attacks on the communication. These kind of attacks are a very real threat and there has been countless examples of them. Hop-by-hop security is sometimes also referred to as point-to-point security. 

##### End-to end security
The second pattern, _end-to-end_ security, visualised in Figure 2, is to not terminate the session at the proxy but instead keep the channel security enabled through the proxy. This thwarts the possibility for the proxy to attack the session in a meaningful way since it prevents it from reading the data or changing it without detection. 

![End-to-end security]({{ site.url }}/images/e2e_900.png)

**Figure 2**

True end-to-end security is thereby obtained but important functionality is also lost. With channel security used for end-to-end encryption, it is all or nothing; all data originating from above the session layer has to be secured. The inability for a proxy to change or read anything from the transport layer and higher layers is not without negative consequences. A proxy often carries a lot of functionality on higher layers that is broken by end-to-end channel security. For example, a CoAP caching proxy can not cache any data for connections that tunnels through the proxy using channel security. CoAP is a protocol designed to work closely with proxies; the protocol will be crippled without the proxying functionality.

#### Advantages of transitioning to Object Security
When the end-to-end security is based on object security instead, the protocol can pick and choose what part of the data that should be confidential or integrity protected. For example it would be possible to encrypt the payload, integrity protect static parts of the header and leave variable header parts be. 

Protecting certain parts of the header can be very important. A malicious intermediate tampering with the header can do considerate damage. For example an attacker could change the _method code_ from **GET** to **DELETE**, thereby deleting a resource instead of fetching the current value. This is one reason for why encryption of just the payload of the CoAP message is not sufficient. 

In conclusion, object security enables end-to-end security without preventing proxy operations, if done correctly.

### Network Overhead
Another important aspect when comparing object security and channel security for constrained devices is the network overhead produced by the different technologies. The overhead in a session based security protocol is composed of both the handshake and the overhead that the protocol produces when encrypting and integrity protecting data. 

In an object based security protocol, the encryption parameters can be pre established (I will discuss Perfect Forward Secrecy in a coming blog post), in which case the overhead consists solely of the overhead produced when encrypting and integrity protecting, no handshake takes place. 

### Conclusion and proposed solution
This blog post has argued that using channel security protocols, such as DTLS, for constrained nodes is often not the best solution. Protocols based on object security can often be a much better fit. 

One of the most popular IoT protocols targeting constrained devices is [CoAP](https://tools.ietf.org/html/rfc7252). In the master thesis written by me and Martin Gunnarsson, we explored the feasibility of using the novel [OSCoAP](https://tools.ietf.org/html/draft-selander-ace-object-security-05) protocol draft for securing constrained devices.

OSCoAP is an end to-end object security protocol which aims at reducing the overhead and complexity by adding object security as an option in CoAP packets. In the master thesis the protocol was implemented and tested against the current state of the art of channel security, DTLS. The results show great promise.


