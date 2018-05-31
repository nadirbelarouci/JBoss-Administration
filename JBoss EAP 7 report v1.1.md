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

## Messaging features 

## Distrubted Cache features 

# Managed domain vs standalone server 
