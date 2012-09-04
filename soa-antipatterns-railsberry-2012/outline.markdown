<script>document.write('<script src="http://' + (location.host || 'localhost').split(':')[0] + ':35729/livereload.js?snipver=1"></' + 'script>')</script>

# SOA AntiPatterns

##### Rantings by...

### Tammer Saleh

#### (...of Thunderbolt Labs)

# Who am I?

## Tammer Saleh

* Detected Earthquakes in Southern California
* Wrote Hoptoad and Shoulda at Thoughtbot
* Wrote Rails AntiPatterns
* VP Engineering at Engine Yard
* Cofounder of...

# Thunderbolt Labs

[LOGO]

# Modern SOAs

## Forget SOAP 

[picture soap]

## Forget Layers

[picture layer cake or marketecture diagram]

Vertical separations are as common as horizontal.  It's about OOP.

# OOP SOA

### The best techniques from Object Oriented Programming can be applied to SOAs.

* Single responsibility 

  (Do only one thing, but do it well)

  [swiss army knife]

* Encapsulation 

  (Respect privacy)

* Data abstraction 

  (All data should be passed in common formats)

## Caveat! In SOA, performance is immediately important.

# AntiPatterns!

(You're doing it wrong.)

[train wreck]

# Writing Internal APIs

[diagram internal api]

## Custom wire protocol

Fear not-invented-here! Many strong and efficient protocols exist:

* JSON
* BSON
* Protocol Buffers
* BERT
* MessagePack

## We don't need an Architecture!  We have an SOA!

[ugly house]

You cannot write an SOA without understanding the larger picture.  Each service needs to be viewed in relation to the overall system.  The system, in turn, needs to be viewed as a single entity.

## Tower of Babel

[tower of babel]

Each service is implemented in isolation, with different URL patterns, transport protocols, and failure modes.

[orchestra]

Ideally, each service uses the same language and framework underneath to facilitate massive code reuse and consistency.

## Centralized DB

[diagram]

Never violate another service's encapsulation.

* Data integrity
* Schema management nightmare
* Scaling bottleneck

## Grand-central API

All of your services depend on a single central service to operate

[diagram]

# Consuming APIs

[diagram api client]

# Testing

## Lack of Real Integration Tests

An SOA must be viewed as a single system.  Tests that exercise the entire system at once are absolutely necessary.

## Each consumer is responsible for mocking each service

A service should provide a trusted mock interface with the client-side gem.

# Performance

## Lack of caching

Even memoization makes a huge difference

## Serial requests

[diagram]

Replace with a Futures pattern 

(Celluloid)

## In-line requests

[diagram]

Replace with either fire-and-forget or background processing.

## Rollercoaster traffic spikes

[diagram queue]

Amortize traffic through use of queues.  Similar to fire and forget on client side.

# Reliability

## Tripwire clients

OMG A SERVICE IS DOWN!!!?!

## James Golic

[jamesgolick]

* Rollout
* Degrade

https://github.com/jamesgolick

## Chaos Monkey

[chaos monkey]
