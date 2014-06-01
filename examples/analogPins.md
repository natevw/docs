# Analog Pins

Tessel features 6 Analog Pins and 3 PWM-capable pins on the `GPIO` bank. `A1`-`A6` are able to read analog values and `A1` is a digital-to-analog converter, meaning it can write a true analog value. `G4`-`G6` have the ability to output a PWM value. This is useful for sending signals to things like LEDs and servos.

## Reading an Analog Pin
Pins `A1` - `A6` can read an analog value between 0 and 3.3V. The returned value is a float between 0.0 and 1.0 where 0.0 represents 0V and 1.0 represents 3.3V. Do not feed more than 3.3V into the ADC.
```.js
/* 
Reads analog pin 6 once a second
*/
var tessel = require('tessel');

// Can use A1-A6
var readPin = tessel.port['GPIO'].pin['A6'];

setInterval(function () {
  console.log(readPin.read());
}, 1000);
```

## Writing an Analog Pin
There are two ways to write an analog value with Tessel: the [digital-to-analog converter (DAC)](http://en.wikipedia.org/wiki/Digital-to-analog_converter) and with [Pulse-Width Modulation (PWM)](http://en.wikipedia.org/wiki/Pulse-width_modulation). `pwm` on Tessel is the same as `analogWrite` on most Arduino boards. However, PWM is not exactly an analog signal and you should check with whatever you're trying to connect to Tessel to choose which kind of output you need. If you're not sure, ask us on [the forums](forums.tessel.io)!

## Writing a PWM value:
```.js
/* 
 Ramps up PWM signal until it's at max duty cycle
 then starts back at 0
*/
var tessel = require('tessel');

// Can use G4, G5, or G6 for PWM
var outputPin = tessel.port['GPIO'].pin['G4'].output();

var speed = 0;

setInterval(function () {

  // Increase the speed by 5%
  speed += 0.05

  // If the speed is at max, go back to min
  if (speed >= 1) speed = 0;

  console.log('writing pwm value of', speed * 3.3);
  // Output that value
  outputPin.pwm(1000, speed);
  
}, 100);
```

## Writing an Analog value
```.js
/* 
 Ramps up a digital signal until it's at max voltage
 then starts back at 0V.
*/
var tessel = require('tessel');

// Can only use A1 for real analog output
console.log(tessel.port['GPIO'].analog[0]);
var outputPin = tessel.port['GPIO'].analog[0];

var speed = 0;

setInterval(function () {

  // Increase the speed by 5%
  speed += 0.05

  // If the speed is at max, go back to min
  if (speed >= 1) speed = 0;

  console.log('writing analog value of', speed * 3.3);
  // Output that value
  outputPin.write(speed);
  
}, 100);

```