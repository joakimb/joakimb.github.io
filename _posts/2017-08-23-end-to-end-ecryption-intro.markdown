---
layout: post
title: "How to classify different security solutions for your needs"
date: 2017-08-04 12:00:00 +0200
---


## Introduction

Security is, and always has been, a basic requirement for quality products. It has been part of every serious system since the start of networked computing in the late 1900:s and is now part of every mans life with the moving of some parts of our lives to the internet. However, it has always been hidden under the surface. It has not been publicly advertised by neither the media or the system builders.

The situation has changed. Nowadays, security breaches is front page news on the most respected newspapers and system builders openly discuss their securiy model as a part of marketing.

## The why (the cause for the new situation)

An important cause for security moving from the back to beeing pronounciated is the booming of clous computing. Most systems are now fully or partly cloud? Why? Because it is easy and csoat efficient. The downside is that the flyffy term "The Cloud" really only means "Someone elses computer". 

Think about it. Saying, "we will get a cheaper and more availabe service using the cloud" sounds much better than, "we can save cost by putting all of our core data on someone elses computer".

Using the cloud means bringin in more parties in the system, which has to be trusted. In the security business, we call that an increased attack surface. When changing the system by adding a ofr example a cloud storage service, your system can now also be attacked in that part of the system.

People should not need to fully trust the service provider and all the accopanyuing parties with their sensitive data. This is where encryption comes in to play.

![Cloud History]({{ site.url }}/images/cloudhist.png)

**Cloud History**

## The how (how is security implemented)


### securing every part of the system

Most people that has come in contact with computer security have heard about the common techniques for connection security and end node security

these are very good technologies. but the purpose is to secure the infrastructure, piece by piece, or as we call it in the industry, hop-by-hop security.
This means that when for example sending an email, the email is encrypted in transit and in storage, but the parties delivering the mail are able to decrypt the message. This ability allows for weaknesses, either if there is a vulnerability at some part of the chain. or if some part of the chain is malicious.

#### Email example for HBH

1. sender creates email and encrypts it for her email server.
2. the senders email server receives the email and decrypts it
3. the senders email server reencrypts the email for therecipients email server and send it there
4. the recipients email server receives the email, decrypts it, and then reencrypts it for the recipient.

here, the security is divided in to several steps, and the email is vulnerable everytime is decrypted and reencrypted.
it is also a system with 4 parties. The sender, the receiver, the senders email service and the receivers email service.
All of these parts needs to be trusted (we wont get into the email services thrird party services since it would be too complex, but basically, those also needs to be trusted.)

### securing the data at the end nodes

securing your infrasturcure is essential for quality, but when the users of a system should not have to fully trust the entire system, here we need the alternative to hop-by-hop, i.e. end-to-end sedcurity.
In end-to-end security, you encrypt for the final recipient at the first step. the data can be delivered through an arbitrary

There are many technologies available for e2e, e.g. HYKER, PGP, Signal and OSCOAP. They all acheive fully secure e2e, but with important difference making them suited for different purposes.

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


![Comparison]({{ site.url }}/images/comparison.png)

**E2EE Comparison**

# Briteback

## the briteback aim

fill in yourselves

## briteback security model

## competition security model
slack, crypho, microsoft, whatsappj
