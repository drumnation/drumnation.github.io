---
layout: post
title: "Making a Dumb Laser Smart by Hacking it to a Raspberry Pi"
date: 2017-12-16 5:39:06 -0400
---

# Smart Lasers are for Super Heroes

Lasers are cool. Lasers you can program to do what you want are even cooler. We had connected with a company that makes a fiber optic laser that beams color through what looks like fishing line.  

The fishing line can be very long and it can be wrapped around objects.  We started off with a generic remote control system that had colors and some lighting sequences programmed into it.  

The goal was to figure out how to make a Raspberry Pi control the laser to do the same stuff.  This way we could wrap that fishing line around a game console at one of our events and program the game itself to trigger the lighting.

Getting hit triggers a red flash for example.

# Here's a video of it in action

[![Smart Laser](/images/smart-laser/youtube.png)](https://youtu.be/FB2o6krFmZw)

# Hardware

I had never used a Raspberry Pi before, but had spent my childhood building and fixing PCs.  It didn't seem that far away from things I already knew. 

Before even touching the laser I approached the problem by looking up Raspberry Pi projects where you use a breadboard to control a very simple LED using the Pi.

Once I was able to turn a light on and off as well as dim and brighten it, I tried to figure out how to make an adapter to connect the laser to the Pi instead of the LED.

Once I was able to get the Pi to control the laser on one channel I added 2 more channels for the remaning primary colors and blended them together to make all the colors of the rainbow.  

This approach worked out well for me as I was able to get this whole thing working in about 4 days.

# Connecting to GPIO
```py
#!/usr/bin/env python
"""Breathing Rainbow Light Program"""

from time import sleep
import math
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BCM)
red = 20
green = 21
blue = 26

GPIO.setup(red, GPIO.OUT)
GPIO.setup(green, GPIO.OUT)
GPIO.setup(blue, GPIO.OUT)

# Configuring Red, Green, Blue - Primary Colors

Freq = 60  # Hz

RED = GPIO.PWM(red, Freq)
RED.start(0)
GREEN = GPIO.PWM(green, Freq)
GREEN.start(0)
BLUE = GPIO.PWM(blue, Freq)
BLUE.start(0)

# Using Voltage Control to Dim the LED

def color(R, G, B, on_time):
    """color brightness range is 0-100"""
    RED.ChangeDutyCycle(R)
    GREEN.ChangeDutyCycle(G)
    BLUE.ChangeDutyCycle(B)
    sleep(on_time)

    # turn everything off
    RED.ChangeDutyCycle(0)
    GREEN.ChangeDutyCycle(0)
    BLUE.ChangeDutyCycle(0)
```

# Using a Breadboard to Connect the Laser to the Pi

In order to send these signals from the raspberry pi back to the laser I stripped wires on a 6 pin SVGA adapter.  The manual for the laser showed separate wires for each color primary color channel as well as a ground, power, and one other wire that didn't seem to be used anywhere.

## Resistors

I connected each primary color to the breadboard as well as the power from the Laser and the ground from the Pi.  I then completed the circuit using a 330 Ohm resister to keep the raspberry pi from blowing up.

Beyond keeping the Pi from exploding the strength of the resistor also effected the color strength and through some trial and error I found this to produce the most accurate color.

![Breadboard Adapter](/images/smart-laser/breadboard2.png)

# Writing a Rainbow Program

```py
def PosSinWave(amplitude, angle, frequency):
    """angle in degrees, creates a positive sin wave between 0 and amplitude * 2"""
    return amplitude + (amplitude * math.sin(math.radians(angle) * frequency))


try:
    while 1:
        for i in range(0, 720, 5):
            colour(PosSinWave(50, i, 0.5),
                PosSinWave(50, i, 1),
                PosSinWave(50, i, 2),
                0.1)
```

# Clean Up

```py
except KeyboardInterrupt:
    pass

RED.stop()
GREEN.stop()
BLUE.stop()

GPIO.cleanup()
```