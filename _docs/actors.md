---
layout: docs
title: FTW Actor Model
permalink: /concepts/actors/
authors:
  - Jack R. Dunaway
editors:
  - placeholder
---

Featherweight provides an Actor Reference Design implementation
for actor-oriented development (AOD) in LabVIEW.

This reference design is not related to NI Actor Framework (NI-AF). The
FTW Actor Model more closely resembles design principles, tenets, and components
from Akka, an actor framework for Scala and Java.

{% include note.html type="warning" text="Note to NI Actor Framework Users: FTW actors are different." %}


## FTW Actor Decomposition

A FTW Actor has several components, some of which are optional:

### Incoming Message Inbox

An Actor Instance has one or more [1, N) Incoming Message Inboxes.
These in

### Job Scheduler

In order for an

### Construction



### Destruction



### Additional Sockets
In addition to the requisite Incoming Message Inbox, an Actor may
implement additional means of communicating via messages with peer nodes.

All of these additional sockets may have [0, N) instances, and no one
precludes any other given sufficient compute resources and messaging
bandwidth.

An Actor may be a Publisher, sending messages on one or more Publisher
Sockets.

An Actor may be a Subscriber, receiving messages on one or more Subscriber
sockets.

An Actor may be a Producer, pushing messages into a message queue.

An Actor may be a Consumer, pulling messages from a message queue.

### Connector
An Actor Instance exposes its "Published API" as a conceptual document
(not as a programmatic API) that defines the wire-level protocol of communication
to and from the Actor. This is a set of the request messages
and their payload structures and the response formats to those requests.

How then may the Actor be invoked? Nominally, by creating an Actor Connector,
which is a binding written in a language to implement the Actor's
Published API.

Additionally, multiple connectors may be written against one single
actor if/when it is desirable to expose unique subsets of the Actor's
full capabilities (nominally, to make programming easier by providing
fit-for-purpose APIs based on application domain).

{% include note.html type="protip" text="An Actor Instance is decoupled from an Actor Connector, meaning clients are not statically linked to dependencies of the actor. This enables remoting and location transparency, and also makes clients smaller. The tradeoff is a slightly larger codebase, and the possibility to introduce developer errors due to string-typing." %}

## FTW Actors vs. NI Actor Framework

The NI Actor Framework (NI-AF) is the most important reference design to
exist in the LabVIEW community since the JKI State Machine.

NI-AF represents the first widely-acknowledged need to work together
on an Application Framework for LabVIEW because 2) there 

## FTW Actors vs. QMH

One common design pattern in LabVIEW is the Queued Message
Handler (QMH). There are multiple reference designs from several
vendors including one that ships as a LabVIEW Project Template Library.

The NI Actor Framework is an object-oriented reference design
of a Queued Message Handler, but with object-oriented encapsulation
and type safety.

The QMH pattern enables asynchronous loops to communicate
with one another via messaging, and it also enables arbitrary sequencing
of execution order within one loop.

The message queue and job queue of the QMH in most reference designs
(including NI-AF) are physically the same queue. This means that messages
from peers are not "requests", but rather "commands".

One advantage to combining the queues is simplicity to develop and debug.
One disadvantage is that the simplicity is suited for simple systems, and
does not readily scale to more complex or nuanced systems.

Take for example Nagle's algorithm in networking. It states that many small
finite bits of work will be aggregated for a period of time before dispatching
a larger chunk of work, because the overhead of each atomic piece
of work is the same and considered expensive when performed in small
pieces.

Most QMH reference designs do not 

## FTW Actors vs. 

## Incoming Message Queue vs. Job Queue



## Actor Model Definitions

* **remote actor** - from the perspective of one actor, the other actor. Generally,
  we reserve "remote actor" when physical distance over a network separates the two actors.
  If it's another actor in the same Actor Context, we might simply say "other actor".
  They're virtually interchangeable with a twinge of connotation, since an Actor Instance
  is not concerned with concrete implementation details of the message transport connecting.
* **worker pool**:

##

## Poison Pill



{% include note.html type="warning" text="" %}
