## (sketch slides)

---

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

# Meetups

---

# Mega Meetups

---

# Super Mega Meetups

---

![fit](day-of-rest-assets/msmm.png)

---

[i feel like these are important - it's really valuable to learn about new perspectives]

[tech communities tend to be in little bubbles - you can gain a lot from getting a new perspective]

[new perspectives are as important]

---

![fit](day-of-rest-assets/wpox.png)

---

# ~

---

# What I'm going to talk about today:

---

# What I'm going to talk about today:

# [fit] Things

---

## [fit] _The Internet of Things_

## [fit] 1. Connecting things together
## [fit] 2. How to design things
## [fit] 3. Making use of things
## [fit] 4. How to build useful things

---

# ~

---

# 1
# [fit] Connecting things
# [fit] together

---

# Some example things

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
* Understanding
* Chunked encoding
* Persistent connections
* IP address changes

---

# JSON is pretty challenging too

![inline](day-of-rest-assets/parse-json.png)

---

# Small devices

* sketchy/variable network
* low processing power
* power consumption requirements

---

<!--# [fit] HTTP / JSON / REST-->

# [fit] We're going to need
# [fit] a smaller boat

![inline 100%](day-of-rest-assets/smaller-boat.gif)

---

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

## nope
# [fit] ~~REST~~
# [fit] ~~HTTP~~*

---

(MQTT over WebSockets)

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
POST /btn HTTP/1.1
Host: benjaminbenben.com
User-Agent: curl/7.51.0
content-type: application/json
Accept: */*
Cache-Control: no-cache
Content-Length: 15

{"state":"pressed"}
```

# vs

```
⨽⨽⨽⨽/btnpress
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

# Connecting to wordpress things

---

## MQTT to REST bridge

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
