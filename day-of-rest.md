theme: Zurich, 2

# Hello

---

# Who's used

# [fit] The WP-REST API

---

# Let's assume you're __really__ into

# [fit] The WP-REST API

---

![fit](day-of-rest-assets/last-ador.png)

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

^ 34 places in the US called Oxford

---

![80%](day-of-rest-assets/jsoxford.png)

---

# Evening meet-ups <br/>Hack days

---

# Last month with

# Oxford Python, OxRUG, Codebar, DotNetOxford, DevOpsOxford, Drupal & WPOx

^ 8 groups together

---

# [fit] The Oxford
# [fit] Mega Super
# [fit] Meetup Meetup

---

![fit](day-of-rest-assets/msmm.png)


^
The tech community is great, though sometimes we get stuck in a bit of a bubble
It's really valuable to hear a viewpoint different from your own

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

## 1. Many small (movable) devices - wires become an issue
## 2. Batteries - constrains compute resource
## 3. Things need to be able to talk to each other

^
With everything being connected - wires become a problem
Low power devices may last years on a battery, this means running an http server may be impossible
It's not just about sensors, sometimes you need devices to be able to communicate with each other (windows open when too hot)





---

# [fit] Part 1.
## A __deep dive__ into IoT networking, and it can fit with WordPress

^
By the end of this - I hope you'll have a DEEP Understanding of a particular IoT network and how it can fit with other stuff.

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

### (wherever they are)

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

^ The word Telemetry means to get information from somewhere and receive it somewhere else

<!-- ---

* 1999 (with wireless in mind)
* lightweight
* publish subscribe topics
* message redelivery -->

---

![cover](day-of-rest-assets/sat.jpg)

^
MQTT was built with a use case in mind
Sensors on an oil pipeline wanting to transmit data through a satellite uplink
Satellites orbit the earth, so they might come in and out of contact and that's completely expected.

---

![fit](day-of-rest-assets/broker.pdf)

---

### publish subscribe
### topics
### messages

![fit](day-of-rest-assets/broker.pdf)

---

# MQTT Broker

* Could be a satellite
* Could be a hub in a home
* Could be a box sitting in a field

^
A broker could be many things:
A satellite
A central hub in a smart home
Sitting in a field

---

# MQTT Connection

* Satellite radio
* mesh network
* bluetooth LE

^
The connecction is tcp/ip based, but fairly flexible
Zigbee mesh network
BLE services

---

# MQTT Connection

* Stateful
* Message based
* Binary

^ you've got a persistent connection and you're sending binary messages back and forth along it.


---

# Message structure

```
        +----------------------+
        |   fixed header       |
        |----------------------|
        |   variable header    |
        |----------------------|
        |   payload            |
        +----------------------+
```

---

# Message Types

## CONNECT, CONNACK, PUBLISH, PUBACK, PUBREC, PUBREL, PUBCOMP, SUBSCRIBE, SUBACK, UNSUBSCRIBE, UNSUBACK, PINGREQ, PINGRESP & DISCONNECT

^
There are 14 types of message, and that's it
but tonnes compared to HTTP PUT GET etc
* these are at a higher level
* HTTP actually has way more

---

# [fit] Connecting to a broker
## ie. making initial contact
## CONNECT & CONNACK

---

```
   Client                     Broker

       ------- CONNECT ------->


       <------ CONNACK --------

```

---

# CONNECT fixed header

![original inline 70%](day-of-rest-assets/packet-fixed-header.pdf)

---

# CONNECT variable header

* protocol number
* whether username & password are supplied

---

# CONNECT payload

* the actual username & password (if required)

---

# 'fourteen bytes' to connect

* 2 bytes - fixed header
* 12 bytes - variable header
* 0-n bytes - flag values

---

# [fit] Publishing Messages

## eg. on button press

# PUBLISH

---

# PUBLISH fixed header

![original inline 70%](day-of-rest-assets/packet-fixed-header.pdf)

---

# PUBLISH variable header

![original inline 70%](day-of-rest-assets/packet-topic.pdf)

---

# PUBLISH payload

- the content of your message (**optional**)


## publish /button/push **'count: 5'**

---

```
⨽⨽⨽⨽/button/push
```

---

```
⨽⨽⨽⨽/button/push
```

# vs

```
POST /button HTTP/1.1
Host: benjaminbenben.com
User-Agent: curl/7.51.0
content-type: application/json
Accept: */*
Cache-Control: no-cache
Content-Length: 15

{"action":"push"}
```

---

# 16 bytes

---

# 16 bytes

# [fit] POST /btn HTT

---

# [fit] Receiving Messages

## eg. some lights

# SUBSCRIBE

---

# SUBSCRIBE fixed header

![original inline 70%](day-of-rest-assets/packet-fixed-header.pdf)

^
the same!
THOUGH - QOS is actually 1 now and we'll get to that

---

# SUBSCRIBE variable header

- Message identifier

^ need a message identifier because QOS is one

---

# SUBSCRIBE payload

- a list of topic names

^ you give the length of each name, followed by the name (and QoS)

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

# Receiving messages

---

# Receiving messages
### (we already know this!)

```
   Client                     Broker

       <------ PUBLISH --------
```

---

# Job Done
# We can publish & subscribe to a network of things

---

```php
$socket = fsockopen('mqtt-host.com', 1883);

$connect = constructConnectMessage();
fwrite($socket, $connect, strlen($connect));

$publish = constructPublishMessage('/hello', 'world!');
fwrite($socket, $publish, strlen($connect));
```

---

## One other thing:

# [fit] QoS

## For when you're not sure if your satellite is even there

^ This is a feature that makes mqtt different from http requests, you can control how your message gets delivered

---

![80%](day-of-rest-assets/packet-fixed-header-qos.pdf)

---

# QoS 0
## Fire and Forget


```
   Client               Broker              Client

       ---- PUBLISH ---->

   [delete]              ---- PUBLISH ---->
```


---

# QoS 1
## At least once delivery

```
   Client               Broker              Client

       ---- PUBLISH ---->

                       [store]
                            ---- PUBLISH ---->
                       [delete]

       <------ PUBACK ---
  [delete]
```

---

# QoS 2
## "Exactly once"


```
   Client               Broker              Client

      ----- PUBLISH ---->
                       [store]
                            ---- PUBLISH ---->
      <------ PUBACK ----
      ------- PUBREL --->
                       [delete]
      <------ PUBCOMP ---
  [delete]
```

---

## Embraces flakey networks

---

# [fit] MQTT
## …that's about it

### (security/encryption, client IDs, last will, persistence, ping/pong)

---

# Demo

## Let's hook these lights together

---

# /Demo

---

## Bringing WordPress onto the network

---

## 1/ Subscribe to MQTT topics
## 2/ Publish messages to MQTT

---

## 1/ Subscribe to MQTT topics

# Create an mqtt-wp bridge

### (A script that forwards on messages)

### [github/benfoxall/mqtt-wp](https://github.com/benfoxall/mqtt-wp)

---

```js
const WPAPI = require('wpapi')

const wp = new WPAPI(config.wp)

const update = (slug, content) =>
  wp.pages().slug(slug)
    .update({content})


// update('my-thing', 'Sensor value: 4')
```

---

```js
const mqtt = require('mqtt')

const client = mqtt.connect(config.mqtt_host)

client.subscribe('my/sensor')

client.on('message', (topic, buffer) => {
  const content = escape(buffer.toString())
  update('my-sensor', content)
})

```

---

# Publishing messages

---


# Option 1:

Via WP-MQTT
➡︎ Nice and easy


# Option 2:

Via Webhooks
➡︎ Better fit for a wp-bridge model

---

## WP-MQTT settings

![fit original](day-of-rest-assets/wp-mqtt-1.png)

![fit original](day-of-rest-assets/wp-mqtt-2.png)

---

![](day-of-rest-assets/wp-mqtt.pdf)

---


# Demo

## Let's bring WordPress on to the network

---

# ~

---

# More useful things with this

* Displaying sensor content
  * weather sensors
  * door opening counters
  * whatever you can build

---

# More useful things with this

* A device registry
  * Smart home
  * Sensors in the field

---

# ~

---

# Doing this at home
## MQTT brokers

---

![fit left original](day-of-rest-assets/host-mosquitto.png)
![fit right original](day-of-rest-assets/host-mosquitto-demo.png)

---

![fit](day-of-rest-assets/host-emq.png)

---

![fit](day-of-rest-assets/host-aws.png)

^
The rest of the AWS ecosystem
Kinesis, stream processing
Lambda, webhooks would allow you to do this at scale

---

![fit](day-of-rest-assets/host-cloudmqtt.png)
![fit](day-of-rest-assets/host-hivemq.png)

---

# Doing this at home
## Electronics

---

![](day-of-rest-assets/puck.jpg)

^
This is the device that I've used for my two things.
This is a wonderful device. Small, looks nice.
BLE only at the moment, though I wrote a BLE to MQTT bridge

---

## Espruino

![original 30%](day-of-rest-assets/espruino.png)

---

# ESP8266

![original](day-of-rest-assets/ESP8266-ESP-12.jpg)

---

# Pi Zero W

![original](day-of-rest-assets/pi-w-adafruit.jpg)


---

# ~


---

# [fit] Part 2.

# How can we take inspiration from the way we build Things.


^ We've now seen that wordpress can be a *thing*

^ Let's think of some properties of things, and how we can use them to inspire us in software development

---

# A Thing

---

# A Thing
<!--# Is more than what it's made from-->
# Is more than the stuff it's made from

^ If you put some stuff together, you've got a new thing. Not just stuff

---

# ~~A wooden table~~

# A table

^
We then refer to it by the thing that we're created
You don't say "come and sit at the wooden table", you say, come and sit at this table.
What is important is that there's a table, not what it's made from.
However, it can take great skill to turn wood into a table, but I'd say that's more to do with craft of a carpenter rather than there being a carpenter in the first place.

---

# [fit] We build applications
# [fit] from source code

```php
print '<b>hello <i>world</B></i>';
```

^
It can be tempting to think of writing code is our job, when actually we're creating the thing that code does.
And again, it takes great craft to write good code but it's not why we do it.

---

# Working out what you're trying to build

^
Because we work in digital, it can be particularly hard to define what the thing we're building is; rather than a table say, which is pretty obvious that it exists.
This is especially difficult in larger teams.

<!---
# [fit] Our job is about
# [fit] creating stuff
# [fit] code is out tool for doing that
-->

---

<!-- # [UX & Wireframes] -->

![cover](day-of-rest-assets/wireframe.jpg)

^
one tool for this is wireframes & diagrams, which can be really useful for getting an early feel for what we're trying to do.

<!--https://www.flickr.com/photos/benoitmeunier/6384895413 -->

---

![fit](day-of-rest-assets/nophone.png)

^
I think this can be taken a bit further though. We're stuck thinking that the thing we're building is an app, or website.
When we might be able to think higher than that.
This is the nophone, it's a bit of plastic, and it's features are that it can't ring.
It's really interesting to start from here, and imagine what a user is trying to do.

---

# Try and work out what your thing does

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

# Someone needs to understand

^
When you create a thing, you create a cognitive model for the person using it.
And this isn't limited to physical things, it can be an idea, or an API or something else.

---

![](images/compact-cassette.jpg)

^ this object holds music

---

![](images/tapes.jpg)

---

![](images/el3302.jpg)

^ Phillips EL 3302 (this one)
^ Phillips EL 3300 - 1963 first ever tape player

^
Microphones - because premastered tapes were hard to get hold of


---


![](images/walkman.jpg)

^
79 - 16 years later - tape players looked like this
Sony introduced the walkman, and it changed the way we experience music

^ this innovation is possible because the tape format was consistent

---

![](images/horizons.jpg)

^
it was also generic enough that years later it could be used to load games.
zxspectrum - floppy drive cost 1000k, or people could just use their tape

---

![](images/tape-adaptor.jpg)

^
This simplicty also paved the way for the future.
This is one of my favourite devices, it bridges the gap between two generations of device. Tapes and CDs



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

---

# [fit] Part 3.
<!-- # How we can build things with the web plaftorm -->
<!--# How the web platform can make our websites into Things.-->
# How we can make our phones into things

----

# MQTT over WebSockets

# Our device becomes another thing on the network

```js
const mqt = new MQT('test.mosquitto.org:8080')
mqt.subscribe('/sensor/value' (value) =>
  document.querySelector('#sensor').textContent = value
)

mqt.publish('/phone/visit', document.location.pathname)
```

---

# [fit] bit.ly/ADORB

---

# Browsers can do more than displaying documents

---

# [fit] Power

```js
navigator.getBattery()
  .then(({value, charging}) => {})
```

---

# [fit] Location

```js
navigator.geolocation.getCurrentPosition(
  ({coords}) => {}
)
```

---

# [fit] Orientation & movement

```js
document.addEventListener('deviceorientation',
  ({alpha, beta, gamma}) => {}
)
document.addEventListener('devicemotion',
  ({acceleration}) => {}
)
```

---

# [fit] Ambient Light

```js
window.addEventListener('devicelight',
  ({value}) => {}
)
```

---

# [fit] Proximity

```js
window.addEventListener('userproximity',
  ({near}) => {}
)
```
---

# [fit] Audio/Video feed

```js
navigator.mediaDevices.getUserMedia({
  audio: true, video: true
})
  .then(stream => {})

```

---

# [fit] Movement

```js
navigator.vibrate(100)
```

---

# [fit] Sounds

```js
var ctx = new AudioContext()
var osc = ctx.createOscillator()
osc.connect(ctx.destination)
osc.frequency.value = 300
osc.start()
```


---

# Other features

---

# [fit] Service <br>workers

```js
navigator.serviceWorker.register('/sw-test/sw.js')
```

---

# [fit] Web <br>Bluetooth

```js
navigator.bluetooth.requestDevice({
  filters: [{
    services: ['heart_rate'],
  }]
}).then(device => device.gatt.connect())
```

---

# Our web pages are becoming Things

---

---

# [fit] Part 4.
# How to build things that make a difference

---

# [fit] We're building things

---

# [fit] The usefulness of <br>a thing can be<br>assessed

---

![fit](day-of-rest-assets/last-flood-map.png)

^ warns people if flood is going to happen

---

![fit](day-of-rest-assets/last-gerard.png)

^ Make me get on a bike and cycle

---

![fit](day-of-rest-assets/last-ador.png)

^ Bring us all here together today

<!-- ---

# Don't worry about page views, shares or likes

---

# Don't worry about page views, shares or likes

# Try to make a difference for people -->

---

# [fit] When you're <br/>building <br/>something

---

# [fit] make<br>a difference<br>for someone

---

# [fit] Thank you <br/>for listening

## @benjaminbenben

---



- [github.com/benfoxall/mqtt-wp](https://github.com/benfoxall/mqtt-wp)
- [github.com/benfoxall/ador-puck-demo](https://github.com/benfoxall/ador-puck-demo)
- [github.com/benfoxall/puck-mqtt](https://github.com/benfoxall/puck-mqtt)

<!--
Photos:

SR-71:
* https://commons.wikimedia.org/w/index.php?curid=54004278
* https://commons.wikimedia.org/w/index.php?curid=30816
* https://commons.wikimedia.org/w/index.php?curid=25099554
* https://commons.wikimedia.org/w/index.php?curid=3492838




Photos:

* By USAF / Judson Brohmer - Armstrong Photo Gallery: Home - info - pic, Public Domain, https://commons.wikimedia.org/w/index.php?curid=30816
* By National Museum of the USAF, imagery by Lyle Jansma, Aerocapture Images - National Museum of the USAF website - linked on Cockpit360 images page (archived 2016-12-12), Public Domain, https://commons.wikimedia.org/w/index.php?curid=54004278
* By USAF/Brian Shul - Shul, Brian (1994). The Untouchables. Mach One. pp. 113–114. ISBN 0929823125., Public Domain, https://commons.wikimedia.org/w/index.php?curid=25099554

* By Frontier India Defense and Strategic News Service - http://album.frontierindia.net/main.php?g2_itemId=112, CC BY-SA 2.5 in, https://commons.wikimedia.org/w/index.php?curid=3492838

* By Jaydec at English Wikipedia, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=32480411

* teapot By Marshall Astor (http://www.marshallastor.com/) - http://www.flickr.com/photo_zoom.gne?id=352811902&size=o (http://www.flickr.com/photos/lifeontheedge/352811902/), CC BY-SA 2.0, https://commons.wikimedia.org/w/index.php?curid=1743696 -->
