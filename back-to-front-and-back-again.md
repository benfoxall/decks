theme: Next, 4

# [fit] Back to front
# [fit] and back again

---

# I'm Ben

---

# Oxford

---

# Woodford

---

#### …
<!--
# The Web
## &
# Other Stuff
-->

# Our Technology
## ⇕
# Our Environment

---

#### Today
# Web Browsers
## ⇕
# LED Fairy Lights

---

# Web Browser

---

# The web

^ Pillars
1. URLS
2. Hypertext
3. Hyperlinks

---

# WorldWideWeb

#### First ever web browser

^ 1st ever web Browser

---

# Nicola Pellow

---

# Line Mode Browser

#### Second ever web browser

---

## NeXT vs Everyone else

---

# LEDs

---


![](images/leds/LED.png)

^ Light Emitting Diode
^ Show CR2032 + LED

---

# PWM

---

![](images/leds/pico.jpg)

---


![](images/leds/rgbled.jpeg)

---

![](images/leds/ws2811.png)

---

![](images/leds/ws2811-chain.png)

---

## Web Browser → LEDS

---

![](images/leds/nodemcu.jpg)

---

```http
POST /

color=%ffcc0000
```

...

```http
200 OK
```

---

```html
<form action="http://10.0.0.15" method="POST" >
  <input name="color" type="color">
  <input type="submit">
</form>
```

---

```
Accept:text/html,application/xhtml+xml,application/xml; \
q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding:gzip, deflate, br
Accept-Language:en-US,en;q=0.9
Cache-Control:no-cache
Connection:keep-alive
Content-Length:15
Content-Type:application/x-www-form-urlencoded
DNT:1
Host:10.0.0.15
Origin:http://localhost:3000
Pragma:no-cache
Referer:http://localhost:3000/
Upgrade-Insecure-Requests:1
User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) \
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36

color=%ffcc0000
```
---

## TCP vs UDP

---

```js
const dgram = require('dgram')
const client = dgram.createSocket('udp4')
const colour = String.fromCharCode(255, 150, 0)
client.send(Buffer.from(`FILL ${colour}`), 8080, 10)
```

---


```
GET /socket HTTP/1.1
Host: localhost
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Protocol: fairy-lights
Sec-WebSocket-Version: 13
Origin: http://example.com
```

---

Browser → Server →* MCU → Fairy Lights

---

# Demo

```html
<input type="color" />
```

---


### Improving our abstraction
# lights.json


---

# Demo

```html
<ol>
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
</ol>
<style>
  li {color: #000}
</style>
```

---

Model -> JS <-> HTML → Server →* MCU → Fairy Lights

---

### Improving our abstraction
# Space

---

## ThreeJS

```js
const mesh = new THREE.SphereGeometry(size, 8, 8)
const material = new THREE.MeshBasicMaterial(
  { color: Math.random() * 0xffffff }
)
```

![inline fill](images/leds/threejs.png)

---

## WebGL

#### attributes + uniforms → vertex shader → fragment shader → draw calls

---

## GPU vs CPU

---

# Demo

### Mouse, ambient, spheres

```js
mesh.forEach((c, i) =>
  c.material.setColor(
    i < mouse ? 0 : 0xffcc00
  )
)
```

---

Model -> JS -> WebGL → Server →* MCU → Fairy Lights

---

### Interaction

---

# Demo

### Audio, leap motion

```js
meshes.forEach(mesh => {
	const d = mesh.position.distanceTo({x:0, y:0, z:0})
	const idx = Math.floor((d/max) * dataArray.length)
	const value = frequencies[idx]
	const {r, g, b} = colourFn(value)

	mesh.material.color.setRGB(r, g, b)
})
```


---

Model -> JS -> WebGL → Server →* MCU → Fairy Lights

---

# The abstraction was for development

---

### Improving our abstraction
# Toward a goal

---

## WebGL

#### attributes + uniforms → vertex shader → fragment shader → draw calls

---


# Demo

### Shader

```glsl
precision mediump float;
varying vec3 vPosition;
uniform float t;

void main () {
  float d = length(vPosition);

  gl_FragColor = vec4(
    sin(d + t),
    cos(d - t) * 0.1,
    tan(d + t),
    1.0
  );

}
```

---

## Expanding an abstraction

---

# [fit] [ screen ]

---


# Demo

### Shader2

```glsl
precision mediump float;
varying vec3 vPosition;
uniform float t;

void main () {
  float d = length(vPosition);

  gl_FragColor = vec4(
    sin(d + t),
    cos(d - t) * 0.1,
    tan(d + t),
    1.0
  );

}
```

---

# In summary

---

# 1. Choose your abstractions

---

# 2. Medium doesn't matter with the web

---

## Thank you for listening

# Ben Foxall

### @benjaminbenben

<!--
Demos:

* post directly to lights ** 2
  * POST / 255 0 0
  * POST /1 0 255 0
* link up to CSS
* HTTP & UDP & Websockets
* ThreeJS
* WebGL ** 1
* Hyperlinks

-->
