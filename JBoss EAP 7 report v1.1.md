# What's new on JBoss EAP 7: 

JBoss EAP 7 includes some notable upgrades and improvements over the previous release.

## Java EE 7

JBoss EAP 7 is a certified implementation of Java EE 7, meeting both the Web Profile and the full
platform specifications. It also includes support for the latest iterations of CDI 1.2 and Web Sockets 1.1.

## Undertow

Undertow is the new lightweight, flexible, and performant web server included in JBoss EAP 7,
replacing JBoss Web. Written in Java, it is designed for maximum throughput and scalability. It
supports the latest web technologies, such as the new HTTP/2 standard.

## Apache ActiveMQ Artemis

Apache ActiveMQ Artemis is the new JBoss EAP 7 built-in messaging provider. Based on a code
donation from HornetQ, this Apache subproject provides outstanding performance based on a
proven non-blocking architecture.

## IronJacamar 1.2

The latest IronJacamar provides a stable and feature rich support for JCA and DataSources.

## JBossWS 5

The fifth generation of JBossWS is a major leap forward, bringing new features and performance
improvements to JBoss EAP 7 web services.

## RESTEasy 3

JBoss EAP 7 includes the latest generation of RESTEasy. It goes beyond the standard Java EE
REST APIs (JAX-RS 2.0) by providing a number of useful extensions such as JSON Web Encryption,
Jackson, JSON-P, and Jettison.

## OpenJDK ORB
JBoss EAP 7 replaced the JacORB IIOP implementation with a downstream branch of the OpenJDK
ORB, leading to better interoperability with the JVM ORB and the Java EE RI.

##Feature Rich Clustering

Clustering support was heavily refactored in JBoss EAP 7 and includes several public APIs for
access by applications.

## Port Reduction
By utilizing HTTP upgrade, JBoss EAP 7 has moved nearly all of its protocols to be multiplexed over
just two HTTP ports: a management port (9990), and an application port (8080).

## Enhanced Logging
The management API now supports the ability to list and view the available log files on a server, or
even define custom formatters other than the default pattern formatter. Deployment’s logging
setup is also greatly enhanced.
For a complete list of new features introduced in JBoss EAP 7.0, see New Features and Enhancements
in the JBoss EAP 7.0.0 Release Notes.

# New Features and Enhancements in JBoss EAP 7.1

## Elytron

Elytron, based on the WildFly Elytron project, is the new security framework in JBoss EAP 7.1. It is
designed to unify security across the entire application server.

## Management Console

The management console has been improved to provide the ability to configure more subsystems,
provide enhanced transaction subsystem and transaction resource metrics, and manage many
additional configurations.

## Management CLI

The management CLI provides enhanced support for response and file attachments, module
configuration, and debugging support through the echo-command argument

# Pre-configured features: 

## High Availability Clustering features

JBoss EAP 7  supports two features which ensure high availability of critical Java EE applications:

**Load balancing** allows a service to handle a large number of requests by spreading the workload across multiple servers. A client can have timely responses from the service even in the event of a high volume of requests.

**Failover** allows a client to have uninterrupted access to a service even in the event of hardware or network failures. If the service fails, another cluster member takes over the client’s requests so that it can continue processing.

*Clustering* is a term that encompasses all of these capabilities. Members of a cluster can be configured to share workloads, referred to as load balancing, and pick up client processing in the event of a failure of another cluster member, referred to as failover.

## Messaging features 

### Java Messaging Service 2.0 (JMS)

JMS is a Java API that provides both point-to-point and publish-subscriber messaging styles. JMS also incorporates the use of transactions.

### High Availability

High availability is the ability for the system to continue functioning after failure of one or more of the servers.
A part of high availability is failover which is the ability for client connections to migrate from one server to another in event of server failure so client applications can continue to operate (Live / Backup Pairs).

### Clustering

JBoss EAP messaging clusters allow groups of JBoss EAP messaging servers to be grouped together in order to share message processing load. Each active node in the cluster is an active JBoss EAP messaging server which manages its own messages and handles its own connections.

### Backward and Forward Compatibility

JBoss EAP supports both backward and forward compatibility with legacy versions of JBoss EAP that were using HornetQ as their messaging brokers, such as JBoss EAP 6. These two compatibility modes are provided by the JBoss EAP built-in messaging server, ActiveMQ Artemis, that supports the HornetQ’s core protocol.

### Large Messages support

JBoss EAP messaging supports large messages, even when the client or server has limited amounts of memory. Large messages can be streamed as they are, or they can be compressed further for more efficient transferral.

### Flow Control (Producer flow control & Consumer flow control)

Flow control can be used to limit the flow of messaging data between a client and server so that messaging participants are not overwhelmed. You can manage the flow of data from both the consumer side and the producer side.

### Message Grouping

A message group is a group of messages that share certain characteristics:

- All messages in a message group are grouped under a common group id.
- All messages in a message group are serially processed and consumed by the same consumer.

## Distrubted Cache features 

# Managed domain vs standalone server 

Before we dive in the Managed domain or the Standalone Server let's define some terms: 

#### Extentions: 

An extension is a module that extends the core functionality of the server. Extensions are loaded as they
are needed by deployments, and are unloaded when they are no longer needed
An extension register a plugin that cloud be use by application within the application server, however to be used it must be registred to the subsystem as well.
To make it easy for you, extensions are just imports like in Java, importing something doesn't mean that you use it, but it's available for you if you need it.
 

#### Subsystem: 

A subsystem provides configuration options for a particular extension, as we said, if an extensions is an import, than you can view a Subsystem as a Instance of that import, configuring it according to your needs in order to use within some applications. 

#### Profile: 

A collection of subsystem configurations makes up a profile, which is configured to satisfy the needs for
the server.

Now, JBoss EAP can be runned as a Standalone server or as a Managed domain
The Standalone server is just a signle instance of JBoss EAP on its own JVM  on a paricular machine
It configured and managed independetly, application deployed to it and managed independetly, a group of Standalone server instances can be coordianted such that provide a cluster envirnement.
In the other hand The domain mode is a mechanism that allow us to define a group of EAP instnces that are managed as a group.
Concretly, we can identefy three parts in the domain mode: 

#### The Domain Controller:

The domain controller is just an EAP instance, on its own JVM, on its own machine, and yes, It's not a Standalone server, we use an other configuration file to launch the domain controller. The domain controller is the master server which responsible for coordinating host controllers which in return are responsible for coordinating one or more JBoss EAP individual server instances.

#### The Host Controller: 
Concretly the Host controller is also an EAP instance which reside on its own jvm and its own machine, it is responsible for managing the life cycle and deployment for one or more JBoss EAP instances

but wait, does it seem that  the domain controller and the host controller are doing the same thing ? 
tipically the domain controller is a specialized host controller which has additional responsibilities for managing the domain, while the host controller are only responsible for managing one or more EAP instances .


