# What's new on JBoss EAP 7: 

JBoss EAP 7 includes some notable upgrades and improvements over the previous release.

- <b>Java EE 7</b>

JBoss EAP 7 is a certified implementation of Java EE 7, meeting both the Web Profile and the full
platform specifications. It also includes support for the latest iterations of CDI 1.2 and Web Sockets 1.1.

- <b>Undertow</b>

Undertow is the new lightweight, flexible, and performant web server included in JBoss EAP 7,
replacing JBoss Web. Written in Java, it is designed for maximum throughput and scalability. It
supports the latest web technologies, such as the new HTTP/2 standard.

- <b>Apache ActiveMQ Artemis</b>

Apache ActiveMQ Artemis is the new JBoss EAP 7 built-in messaging provider. Based on a code
donation from HornetQ, this Apache subproject provides outstanding performance based on a
proven non-blocking architecture.

- <b>IronJacamar 1.2</b>

The latest IronJacamar provides a stable and feature rich support for JCA and DataSources.

- <b>JBossWS 5</b>

The fifth generation of JBossWS is a major leap forward, bringing new features and performance
improvements to JBoss EAP 7 web services.

- <b>RESTEasy 3</b>

JBoss EAP 7 includes the latest generation of RESTEasy. It goes beyond the standard Java EE
REST APIs (JAX-RS 2.0) by providing a number of useful extensions such as JSON Web Encryption,
Jackson, JSON-P, and Jettison.

- <b>OpenJDK ORB</b>
JBoss EAP 7 replaced the JacORB IIOP implementation with a downstream branch of the OpenJDK
ORB, leading to better interoperability with the JVM ORB and the Java EE RI.

- <b>Feature Rich Clustering</b>

Clustering support was heavily refactored in JBoss EAP 7 and includes several public APIs for
access by applications.

- <b>Port Reduction</b>
By utilizing HTTP upgrade, JBoss EAP 7 has moved nearly all of its protocols to be multiplexed over
just two HTTP ports: a management port (9990), and an application port (8080).

- <b>Enhanced Logging</b>
The management API now supports the ability to list and view the available log files on a server, or
even define custom formatters other than the default pattern formatter. Deployment’s logging
setup is also greatly enhanced.


# New Features and Enhancements in JBoss EAP 7.1

- <b>Elytron</b>

Elytron, based on the WildFly Elytron project, is the new security framework in JBoss EAP 7.1. It is
designed to unify security across the entire application server.

- <b>Management Console</b>

The management console has been improved to provide the ability to configure more subsystems,
provide enhanced transaction subsystem and transaction resource metrics, and manage many
additional configurations.

- <b>Management CLI</b>

The management CLI provides enhanced support for response and file attachments, module
configuration, and debugging support through the echo-command argument

# Pre-configured options in JBOSS EAP 7: 

## High Availability Clustering

### Features 
JBoss EAP 7  supports two features which ensure high availability of critical Java EE applications:

**Load balancing** allows a service to handle a large number of requests by spreading the workload across multiple servers. A client can have timely responses from the service even in the event of a high volume of requests.

**Failover** allows a client to have uninterrupted access to a service even in the event of hardware or network failures. If the service fails, another cluster member takes over the client’s requests so that it can continue processing.

*Clustering* is a term that encompasses all of these capabilities. Members of a cluster can be configured to share workloads, referred to as load balancing, and pick up client processing in the event of a failure of another cluster member, referred to as failover.

### Pre-configured options

*Clustering* is made available to JBoss EAP by the **jgroups**, **infinispan**, and **modcluster** subsystems. The ha and full-ha profiles have these systems enabled, but they will only start up if an application configured as distributable is deployed on the servers.

#### [JGroups](http://www.jgroups.org)

**JGroups** is a toolkit for reliable messaging and can be used to create clusters whose nodes can send messages to each other. It allows you to configure named channels and protocol stacks as well as view runtime statistics for channels.

- JBoss EAP is preconfigured with two JGroups stacks:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;***UDP:*** The nodes in the cluster use UDP multicasting to communicate with each other. **This is the default stack**.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;***TCP:*** The nodes in the cluster use TCP to communicate with each other.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To switch the default JGroups Channel to use TCP just execute the following management CLI command:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`/subsystem=jgroups/channel=ee:write-attribute(name=stack,value=tcp)`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ee channel is default JGroup channel.

- By default, JGroups only binds to the private network interface, which points to localhost in the default configuration. For security reasons, JGroups will not bind to the network interface defined by the -b argument specified during JBoss EAP startup, as clustering traffic should not be exposed on a public network interface.

- The jgroups subsystem contains the `default`, `internal`, `oob`, and `timer` thread pools. These pools can be configured for any JGroups stack.

Thread Pool Name | keepalive-time | max-threads | min-threads | queue-length 
--------------- | -------------- | ----------- | ----------- | ------------
default | 60000L | 300 | 20 | 100 
internal | 60000L | 4 | 2 | 100 
oob | 60000L | 300 | 20 | 0 
timer | 5000L | 4 | 2 | 500 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To configure a JGroups thread pool using the management CLI we can use the following syntax:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`/subsystem=jgroups/stack=STACK_TYPE/transport=TRANSPORT_TYPE/thread-pool=THREAD_POOL_NAME:write-attribute(name=ATTRIBUTE_NAME, value=ATTRIBUTE_VALUE)`

#### [Infinispan](http://infinispan.org)

**Infinispan** is a Java data grid platform that provides a JCACHE(Java Temporary Caching API)-compatible cache interface for managing cached data. The **infinispan** subsystem provides caching support for JBoss EAP. When using a configuration that provides high availability capabilities, the **infinispan** subsystem provides caching, state replication, and state distribution support. In non-high-availability configurations, the **infinispan** subsystem provides local caching support.

- Cache Containers : A cache container is a repository for the caches used by a subsystem. Each cache container defines a default cache to be used. JBoss EAP 7 defines 4 default Infinispan cache containers:

Cache container | Used for | default cache
--- | --- | ---
server | singleton caching | replicated cache
web | web session clustering | distributed cache
ejb | stateful session bean clustering | distributed cache
hibernate | entity caching | local cache for new entity & invalidation cache for updated entity & replicated-cache for last update timestamp for each table [more details](https://mindmajix.com/jboss/configuring-hibernate-cache)

Additional caches and cache containers can be added.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Cache modes:**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Replicated:* Entries written to a replicated cache on any node will be replicated to all other nodes in the cluster, and can be retrieved locally from any node.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Distributed:* Distribution tries to keep a fixed number of copies of any entry in the cache, configured as numOwners. This allows the cache to scale linearly, storing more data as nodes are added to the cluster.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Invalidation mode:* In invalidation, the caches on different nodes do not actually share any data. Instead, when a key is written to, the cache only aims to remove data that may be stale from other nodes.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Synchronous and Asynchronous Replication:* With asynchronous communications, the originator node does not receive any acknowledgement from the other nodes about the status of the operation.

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

## Managed domain vs standalone server 

Before we dive in the Managed domain or the Standalone Server let's define some terms: 

- <b>Extentions</b>

 An extension is a module that extends the core functionality of the server. Extensions are loaded as they
are needed by deployments, and are unloaded when they are no longer needed
 An extension register a plugin that cloud be use by application within the application server, however to be used it must be registred to the subsystem as well.
 To make it easy for you, extensions are just imports like in Java, importing something doesn't mean that you use it, but it's available for you if you need it.
 

- <b>Subsystem</b>

 A subsystem provides configuration options for a particular extension, as we said, if an extensions is an import, than you can view a Subsystem as a Instance of that import, configuring it according to your needs in order to use within some applications. 

- <b>Profile</b>

 A collection of subsystem configurations makes up a profile, which is configured to satisfy the needs for
the server.

 JBoss EAP can be runned as a Standalone server or as a Managed domain.
 The Standalone server is just a signle instance of JBoss EAP on its own JVM  on a paricular machine, it configured and managed independetly, application deployed to it and managed independetly, a group of Standalone server instances can be coordianted such that provide a cluster envirnement.
 In the other hand The domain mode is a mechanism that allow us to define a group of EAP instnces that are managed as a group.
Concretly, we can identefy three parts in the domain mode: 

- <b>The Domain Controller</b>

 The domain controller is just an EAP instance, on its own JVM, on its own machine, and yes, It's not a Standalone server, we use an other configuration file to launch the domain controller. The domain controller is the master server which responsible for coordinating host controllers which in return are responsible for coordinating one or more JBoss EAP individual server instances.

- <b>The Host Controller</b> 

 Concretly the Host controller is also an EAP instance which reside on its own jvm and its own machine, it is responsible for managing the life cycle and deployment for one or more JBoss EAP instances
 
 But wait, does it seem that  the domain controller and the host controller are doing the same thing ? 
tipically the domain controller is a specialized host controller which has additional responsibilities for managing the domain, while the host controller are only responsible for managing one or more EAP instances .

 The domain controller never talks directely to the EAP instance, it must communicate first to the host controller (kind of a proxy).

- <b>The Server group </b>

 A server group is just a group of EAP instances, each EAP instance must belogn to a server group, and a server group can contain EAP instances which are managed by different host controllers .
 
So, <b>what is the difference between standalone and domain mode ? </b>
 None, standalone server and instances in the domain mode offers the same features, you can do the same in either of them, however, we see the difference in how to manage each mode, the standalone is managed independetly while the domain mode manage a group of EAP instances at once! 

### Standalone Server Configuration 
The standalone configuration files are located in the EAP_HOME/standalone/configuration/
directory. A separate file exists for each of the five predefined profiles (default, ha, full, full-ha, loadbalancer).

Configuration File | Purpose
--------------- | -------------- 
standalone.xml | This standalone configuration file is the default configuration that is used when you start your standalone server. It contains all information about the server, including subsystems, networking, deployments, socket bindings, and other configurable details. It does not provide the subsystems necessary for messaging or high availability.
standalone-ha.xml | This standalone configuration file includes all of the default subsystems and adds the modcluster and jgroups subsystems for high availability. It does not provide the subsystems necessary for messaging.
standalone-full.xml | This standalone configuration file includes all of the default subsystems and adds the messaging-activemq and iiop-openjdk subsystems. It does not provide the subsystems necessary for high availability.
standalone-full-ha.xml | This standalone configuration file includes support for every possible subsystem, including those for messaging and high availability
standalone-load-balancer.xml | This standalone configuration file includes the minimum subsystems necessary to use the built-in mod_cluster front-end load balancer to load balance other JBoss EAP instances.


By default, starting JBoss EAP as a standalone server uses the standalone.xml file. To start JBoss
EAP with a different configuration, use the --server-config argument. For example:

`$ EAP_HOME/bin/standalone.sh --server-config=standalone-full.xml`


### Managed Domain Configuration
The managed domain configuration files are located in the <b>EAP_HOME/domain/configuration/</b>
directory.
Configuration File | Purpose
--------------- | -------------- 
domain.xml | This is the main configuration file for a managed domain. Only the domain master reads this file. This file contains the configurations for all of the profiles (default, ha, full, full-ha, load-balancer).
host.xml | This file includes configuration details specific to a physical host in a managed domain, such as network interfaces, socket bindings, the name of the host, and other host-specific details. The host.xml file includes all of the features of both host-master.xml and host-slave.xml, which are described below.
host-master.xml | This file includes only the configuration details necessary to run a server as the master domain controller
host-slave.xml | This file includes only the configuration details necessary to run a server as a managed domain host controller.

By default, starting JBoss EAP in a managed domain uses the host.xml file. To start JBoss EAP with a
different configuration, use the --host-config argument. For example,

`$ EAP_HOME/bin/domain.sh --host-config=host-master.xml`



