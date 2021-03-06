# Galileo-IO

[![Build Status](https://travis-ci.org/rwaldron/galileo-io.png?branch=master)](https://travis-ci.org/rwaldron/galileo-io)

## Galileo-IO is compatible with Intel's Galileo Generation 1, Galileo Generation 2 and Edison boards (Mini and Arduino Board).


Galileo-IO is a Firmata.js-compatibility class for writing Node.js programs that run on the [Intel Galileo](https://www-ssl.intel.com/content/www/us/en/do-it-yourself/galileo-maker-quark-board.html) or the [Intel Edison](http://www.intel.com/content/www/us/en/do-it-yourself/edison.html). This project was built at [Bocoup](http://bocoup.com)

### Getting Started

**As of 0.7.0, only the IoTKit image is supported**

Galileo-IO scripts are run directly from the Galileo or Edison board. To get started, complete the appropriate setup instructions: 

- [Galileo](http://rexstjohn.com/galileo-gen-2-setup/)
- [Edison](http://rexstjohn.com/setting-up-intel-edison-with-intel-xdk/)


### Installation

```
npm install galileo-io
```

or 

```
npm install johnny-five
```


### Usage

This module can be used as an IO plugin for [Johnny-Five](https://github.com/rwaldron/johnny-five).

### Blink an Led

The "Hello World" of microcontroller programming:

(attach an LED on pin 9)

```js
var Galileo = require("galileo-io");
var board = new Galileo();

board.on("ready", function() {
  var byte = 0;
  this.pinMode(9, this.MODES.OUTPUT);

  setInterval(function() {
    board.digitalWrite(9, (byte ^= 1));
  }, 500);
});
```

### Johnny-Five IO Plugin

Galileo-IO is the default [IO layer](https://github.com/rwaldron/johnny-five/wiki/IO-Plugins) for [Johnny-Five](https://github.com/rwaldron/johnny-five) programs that are run on a Galileo or Edison board.

***Note:*** On the Edison, you should require johnny-five first, followed by galileo-io. Otherwise you'll get a segmentation fault.

Example:
```js
var five = require("johnny-five");
var Edison = require("galileo-io");
var board = new five.Board({
  io: new Edison()
});
```

### API

**digitalWrite(pin, 1|0)**

> Sets the pin to 1 or 0, which either connects it to 5V (the maximum voltage of the system) or to GND (ground).

Example:
```js
// This will turn on the pin
board.digitalWrite(9, 1);
```



**analogWrite(pin, value)**

> Sets the pin to a value between 0 and 255, where 0 is the same as LOW and 255 is the same as HIGH. This is sort of like sending a voltage between 0 and 5V, but since this is a digital system, it uses a mechanism called Pulse Width Modulation, or PWM. You could use analogWrite to dim an LED, as an example.

Example:
```js
// Crank an LED to full brightness
board.analogWrite(9, 255);
```

**servoWrite(pin, value)** 

> Set the pin to a value between 0-180° to move the servo's horn to the corresponding position.

Example:
```js
board.servoWrite(9, 180);
```

**digitalRead(pin, handler)** Setup a continuous read handler for specific digital pin.

> This will read the digital value of a pin, which can be read as either HIGH or LOW. If you were to connect the pin to 5V, it would read HIGH (1); if you connect it to GND, it would read LOW (0). Anywhere in between, it’ll probably read whichever one it’s closer to, but it gets dicey in the middle.

Example:
```js
// Log all the readings for 9
board.digitalRead(9, function(data) {
  console.log(data);
});
```


**analogRead(pin, handler)** Setup a continuous read handler for specific analog pin.

> This will read the analog value of a pin, which is a value from 0 to 4095, where 0 is LOW (GND) and 4095 is HIGH (5V). All of the analog pins (A0 to A5) can handle this. analogRead is great for reading data from sensors.


Example:
```js
// Log all the readings for A1
board.analogRead("A1", function(data) {
  console.log(data);
});

```

## License
See LICENSE file.

