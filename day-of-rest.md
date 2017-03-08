theme: Zurich, 2

# Hello

---

# Thanks

---

# I'm Ben

---

![](day-of-rest-assets/card.pdf)

---

![](day-of-rest-assets/card-job.pdf)

---

![](day-of-rest-assets/card-job-2.pdf)

---

# Oxford

---


# Oxford, UK

---

![fit](day-of-rest-assets/oxford-uk.png)

---

![fit](day-of-rest-assets/oxford-us.png)

---

![80%](day-of-rest-assets/jsoxford.png)

---

# Evening Meetups <br/>Hack days

---

# Last month with

# Oxford Python, OxRUG, Codebar, dotnetoxford, DevOpsOxford, Drupal & WPOx

---

# [fit] The Oxford
# [fit] Mega Super
# [fit] Meetup Meetup

---

![fit](day-of-rest-assets/msmm.png)


^ The tech community is great, though sometimes we get stuck in a bit of a bubble
^ It's really valuable to hear a viewpoint different from your own

---

# [fit] HTTP

---

# [fit] HTTP

## [fit] kind of sucks

---

# [fit] HTTP

## [fit] kind of sucks

### (sometimes)

---

# Problem 1.
# HTTP was designed with wires in mind

### wireless/flaky networks can be challenging

^ HTTP assumes a good network

<!--
The idea of requesting a resource, then having to make subsequent RT to populate more information isn't a problem with our sites, it's a problem with our low level api.
GraphQL and h2 are ways of getting round this
-->

---

# Problem 2.
# HTTP resources need to be constantly online
### can be a issue for low-power devices
<!--# HTTP wasn't designed for constrained devices
# Resources have the compute power-->

^ something that it only on for an hour a day, or maybe even a second a day

---

# Problem 3.
# HTTP responses need a request

### getting content to a client can be tricky

---

# The problems:

# 1. Wireless<br/>2. Low energy devices<br/>3. P2P data flow

---

# this sounds a bit like the
# [fit] Internet <br/>of Things

---

# [fit] Part 1.
## A deep dive into IoT networks, and how they can fit with wordpress

---

# An example network of Things

---

<!-- photo note, auto balance, transfer filter -->

![cover](day-of-rest-assets/button.jpg)

---

![cover](day-of-rest-assets/button.jpg)

# 1. a thing you can
# [fit] press down

---

![cover](day-of-rest-assets/lights.jpg)

---

![cover](day-of-rest-assets/lights.jpg)

# 2. some things that
# [fit] light up

---

# target outcome:

# When I press this thing, these things should turn red

---

<!-- curl 'https://benjaminbenben.com/led-strip' -H 'content-type: application/json' -H 'Accept: */*' -H 'Cache-Control: no-cache' --data-binary '{"color":"red"}'  -v -->

Sending a post request to our LED strip?

```
POST /led-strip HTTP/1.1
Host: benjaminbenben.com
User-Agent: curl/7.51.0
content-type: application/json
Accept: */*
Cache-Control: no-cache
Content-Length: 15

{"color":"red"}
```

<!-- content-type is the wrong capitalisation, edge case there -->

---

# Handling HTTP

* Opening ports
* Responding to requests
* Parsing request & headers
* Understanding http codes & header fields
* Chunked encoding
* Persistent connections
* IP address changes

<!--
https://en.wikipedia.org/wiki/List_of_HTTP_header_fields
https://en.wikipedia.org/wiki/List_of_HTTP_status_codes
-->

---

# (JSON is pretty challenging too)

![inline](day-of-rest-assets/parse-json.png)

---

# Small devices

* variable network
* sketchy network
* low processing power
* power consumption requirements

---

<!--# [fit] HTTP / JSON / REST-->

<!-- # [fit] We're going to need
# [fit] a smaller boat

![inline 100%](day-of-rest-assets/smaller-boat.gif)

--- -->

# [fit] MQTT

---

# [fit] **MQ**TT

## Message Queue*

---

# [fit] MQ**TT**

## Telemetry Transport

---

* 1999 (with wireless in mind)
* lightweight
* publish subscribe topics
* message redelivery

---

![cover](day-of-rest-assets/sat.jpg)

---

![fit](day-of-rest-assets/broker.pdf)

---

# MQTT Broker

* Could be a satellite
* Could be a hub in a smart home
* Could be a box sitting in a field


---

# topics, messages, wildcards

![fit](day-of-rest-assets/broker.pdf)


---

# [fit] Connecting to a broker
## ie. making initial contact
## connect

---

```

   Client                     Broker

       ------- CONNECT ------->


       <------ CONNACK --------

```

---

## CONNECT & CONNACK - same fixed header (2 bytes)

![80% inline](day-of-rest-assets/packet-fixed-header.pdf)

---

## CONNECT - additional header

# 12 bytes

* protocol number
* flags (content in payload):
  * last will
  * username
  * password

---

# 14 bytes to connect

* 2 bytes - fixed header
* 12 bytes - variable header
* 0-n bytes - flag values

---

# [fit] Publishing Messages

## eg. on button press

# publish '__/btn__' '__press__'

---

![80%](day-of-rest-assets/packet-fixed-header.pdf)

---

![80%](day-of-rest-assets/packet-topic.pdf)

---

![80%](day-of-rest-assets/packet-payload.pdf)

---

![80%](day-of-rest-assets/packet-example.pdf)

---

```php
fwrite($this->socket,  $message)
```

---

```
⨽⨽⨽⨽/btnpress
```

# vs

```
POST /btn HTTP/1.1
Host: benjaminbenben.com
User-Agent: curl/7.51.0
content-type: application/json
Accept: */*
Cache-Control: no-cache
Content-Length: 15

{"state":"pressed"}
```

---

# 13 bytes

# [fit] POST /btn HTT

---

# [fit] Receiving Messages

## eg. some lights

# Subscribe '**/led/color**'

---

![80%](day-of-rest-assets/packet-fixed-header-message-type.pdf)

---

## Subscribing to a topic

1. Set the message type to 1000 (subscribe)
2. Set payload to '/led/color'

<!--

## Message Types

```
1  CONNECT      Client request to connect to Server
2  CONNACK      Connect Acknowledgment
3  PUBLISH      Publish message
4  PUBACK       Publish Acknowledgment
5  PUBREC       Publish Received (assured delivery part 1)
6  PUBREL       Publish Release (assured delivery part 2)
7  PUBCOMP      Publish Complete (assured delivery part 3)
8  SUBSCRIBE    Client Subscribe request
9  SUBACK       Subscribe Acknowledgment
10 UNSUBSCRIBE  Client Unsubscribe request
11 UNSUBACK     Unsubscribe Acknowledgment
12 PINGREQ      PING Request
13 PINGRESP     PING Response
14 DISCONNECT   Client is Disconnecting
```

-->

---




## MQTT feature:

# [fit] QoS

## For when you're not sure if your satellite is even there

^ This is a feature that makes mqtt very different from http requests

---

![80%](day-of-rest-assets/packet-fixed-header-qos.pdf)

---

# QoS 0
## Fire and Forget

![inline](day-of-rest-assets/qos-temp.jpg)

---

# QoS 1
## "At most once delivery"
## Delivery confirmation

![inline](day-of-rest-assets/qos-temp.jpg)

---

# QoS 2
## "Exactly once delivery"
## Client receive confirmation

![inline](day-of-rest-assets/qos-temp.jpg)

---

## Resilient to flakey networks
## Retries embraced/expected

---

# [fit] MQTT
## …that's about it

### (connect, receipt messages, last will, persistence)

^ you sould prob

---

# Demo

---

## Bringing wordpress onto the network

---

## Expected outcome:

## When we press the button, a page is updated
## When a page is updated, lights flash

---

## An mqtt to wordpress bridge

---

```js
mqtt-to-wp excerpt
```

---

## wordpress to mqtt

---

```js
wp-mqtt
```

---

currently, we're going to use a plugin, though this would be
way better to be exposed by the rest api.  So that the bridge could start and point events at a particular broker

---

# Demo

---

## MQTT to WP-REST

---

# Demo

---

# Keeping things in sync

---

## Webhook to MQTT

# ~

---

## [fit] 2. How to design things


## TODO


---

## [fit] 3. Making use of things

## TODO


---

## [fit] 4. How to build useful things

## TODO


---



# ~~~~



^ Exit - we've now considered wordpress as a "thing".

---

# ~

---

# [fit] Part 2.

# How we can take inspiration from things?

---

# A Thing

---

# A Thing
<!--# Isn't what it's made from-->
# [fit] Is more than it's made from

---
<!--
![](day-of-rest-assets/teapot.jpg)

--->

<!--# [fit] There's more than <br />the implementation-->
# [fit] The implementation <br/>is only part of it

```php
print '<b>hello <i>world</B></i>';
```

---

# [fit] Identifying a goal

<!---

# [fit] Our job is about
# [fit] creating stuff
# [fit] code is out tool for doing that
-->

---

<!-- # [UX & Wireframes] -->


![cover](day-of-rest-assets/wireframe.jpg)



<!--https://www.flickr.com/photos/benoitmeunier/6384895413 -->

---

# As a Team
# We write User Stories
# Because they help keep us focussed

---

![fit](day-of-rest-assets/nophone.png)



<!---

# [fit] People use things
# [fit] to do stuff

--->

---

# Some questions:

## (think of a thing you've built)

---

# Some questions:

# [fit] 1. What does the thing do?

---

# Some questions:

# [fit] 1. What does the thing do? <br/>2. Is it doing it?

^ _This_ is where it could be improved. It's not necessarily about adopting a new standard, or cleaning up code, it's about being purpose driven


<!--
---

# Sometimes we fix solutions instead of problems

-->


---

# A Thing

---

# A Thing
# Is a point of interaction

<!--
Alternatives:
 * Is a concept
 * Has a cognitive model
 * Is a model
 * creates an interface
 * Is an interface
 * Is a cognitive interface
 * Is a point of interaction
-->

---

![](images/compact-cassette.jpg)

---

![](images/tapes.jpg)

---

![](images/el3302.jpg)

^ Phillips EL 3302 (this one)
^ Phillips EL 3300 - 1963 first ever tape player

---


![](images/walkman.jpg)

^ 79 - 16 years later - Sony introduced the walkman

---

![](images/horizons.jpg)

---

![](images/tape-adaptor.jpg)

---

# When you build something

## Make sure it's a thing

---

# A Thing

---

# A Thing

# Doesn't have to be simple

## (but it can be)

---

![](day-of-rest-assets/blackbird-cockpit.png)

^ This is the cockpit of an SR-71, more commonly known as

---

![](day-of-rest-assets/blackbird.jpg)

^ …a blackbird

---


![original](day-of-rest-assets/blackbird-sta.jpg)


---

![original](day-of-rest-assets/blackbird-83k.jpg)

---

![](day-of-rest-assets/blackbird-pilots.jpg)


---

# [fit] The SR-71 is still just a thing

---

# A thing that

# [fit] flies **very** fast<br />& **very** high up

---

<!--

![](day-of-rest-assets/blackbird-cart.jpg)

---

-->

![](day-of-rest-assets/blackbird-fuel.jpg)

^ Starter cart & fuel

---

![fit](day-of-rest-assets/skunkworks.png)

---

![fit](day-of-rest-assets/kelly-johnson.jpg)

^ Kelly Johnson

---

> Keep is Simple Stupid

---

> Keep is Simple, Stupid

---

# Have a clear idea of what your thing is
# Build that thing as simply as you can

---

> For way more about this, see __Skunk Works__ by __Nickolas Means__

---

# A thing

---

# A thing

## Should be a bit magical

---

[magic trick]

---

# Practice the things you

---

# Slight of hand

---

# The things we carry around with us

> INTRO TO NEXT SECTION
> [PHOTO OF PHONE]
> HOW DO WE THINK OF THIS AS A THING


---

[phone picture]

---

Battery

```js
navigator.getBattery()
```

---




# ~

---

---

Possible directions - how the web is changing:
PWA & service workers
- websites are becoming more of a thing.




----


Photos:

* By USAF / Judson Brohmer - Armstrong Photo Gallery: Home - info - pic, Public Domain, https://commons.wikimedia.org/w/index.php?curid=30816
* By National Museum of the USAF, imagery by Lyle Jansma, Aerocapture Images - National Museum of the USAF website - linked on Cockpit360 images page (archived 2016-12-12), Public Domain, https://commons.wikimedia.org/w/index.php?curid=54004278
* By USAF/Brian Shul - Shul, Brian (1994). The Untouchables. Mach One. pp. 113–114. ISBN 0929823125., Public Domain, https://commons.wikimedia.org/w/index.php?curid=25099554

* By Frontier India Defense and Strategic News Service - http://album.frontierindia.net/main.php?g2_itemId=112, CC BY-SA 2.5 in, https://commons.wikimedia.org/w/index.php?curid=3492838

* By Jaydec at English Wikipedia, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=32480411

* teapot By Marshall Astor (http://www.marshallastor.com/) - http://www.flickr.com/photo_zoom.gne?id=352811902&size=o (http://www.flickr.com/photos/lifeontheedge/352811902/), CC BY-SA 2.0, https://commons.wikimedia.org/w/index.php?curid=1743696
