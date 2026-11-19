# Smart Dustbin

An automatic, touch-free dustbin lid-opening system built using an
**Arduino Uno**, an **HC-SR04 ultrasonic sensor**, and an **SG90 servo
motor**.

The project was developed as a **Microprocessor Subject -- Hardware
Project** at **Shri Ramswaroop Memorial University**.

## Project Overview

Ordinary dustbins often require a person to touch the lid or operate a
foot pedal. Frequent contact can spread germs, while lids left open may
attract flies and cause odour.

This Smart Dustbin uses an ultrasonic sensor to detect an approaching
hand or object. When an object comes within the set detection range, the
Arduino commands a servo motor to open the lid. After three seconds, the
lid closes automatically.

## Objectives

-   Enable touch-free operation of a dustbin.
-   Improve hygiene by reducing physical contact with the bin.
-   Build a low-cost system using easily available components.
-   Demonstrate microprocessor concepts such as sensor interfacing,
    timing, and PWM servo control.

## Components Required

  -----------------------------------------------------------------------
  Component                           Purpose
  ----------------------------------- -----------------------------------
  Arduino Uno (ATmega328P, 8-bit AVR, Main controller of the system
  16 MHz)                             

  HC-SR04 ultrasonic sensor           Detects an approaching hand or
                                      object

  SG90 servo motor                    Opens and closes the dustbin lid

  Cardboard                           Used to make the dustbin body and
                                      lid

  Jumper wires                        Connect the sensor, servo, and
                                      Arduino

  Breadboard (optional)               Provides shared 5 V and GND rails

  USB cable                           Powers the board and uploads the
                                      Arduino sketch

  Glue gun and cutter                 Used to cut and assemble the
                                      cardboard bin
  -----------------------------------------------------------------------

## Circuit Connections

### HC-SR04 Ultrasonic Sensor

  HC-SR04 Pin   Arduino Uno
  ------------- ----------------
  VCC           5V
  GND           GND
  TRIG          Digital pin 9
  ECHO          Digital pin 10

### SG90 Servo Motor

  Servo Wire        Arduino Uno
  ----------------- ---------------------
  Red (VCC)         5V
  Brown (GND)       GND
  Orange (Signal)   Digital pin 6 (PWM)

**Important:** The presentation specifies a common GND for all modules
and a 5 V supply from the USB port. Check that your actual wiring
matches the connections above before powering the circuit.

## Working Principle

The system follows a five-step process:

1.  **Trigger:** The Arduino sends a 10-microsecond pulse to the HC-SR04
    TRIG pin.
2.  **Echo:** The sensor emits an ultrasonic sound wave (40 kHz). The
    ECHO signal stays HIGH while the pulse travels and returns.
3.  **Measure:** Arduino's `pulseIn()` function measures the echo
    duration.
4.  **Decide:** The measured time is converted into distance. If the
    distance is greater than 0 cm and less than 20 cm, the lid-opening
    condition is met.
5.  **Actuate:** The servo rotates to 90 degrees, keeps the lid open for
    3 seconds, and then returns to 0 degrees to close it.

### Distance Formula

``` text
Distance (cm) = Time (microseconds) × 0.034 ÷ 2
```

The division by 2 accounts for the sound's round trip between the sensor
and the object.

## Arduino Code

The following sketch follows the program shown in the project
presentation:

``` cpp
#include <Servo.h>

Servo lid;
const int TRIG = 9, ECHO = 10;

void setup() {
  pinMode(TRIG, OUTPUT);
  pinMode(ECHO, INPUT);

  lid.attach(6);
  lid.write(0);  // Lid closed
}

void loop() {
  digitalWrite(TRIG, LOW);
  delayMicroseconds(2);

  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);

  digitalWrite(TRIG, LOW);

  long t = pulseIn(ECHO, HIGH);  // Echo time in microseconds
  float d = t * 0.034 / 2;       // Distance in centimetres

  if (d > 0 && d < 20) {
    lid.write(90);
    delay(3000);  // Keep lid open for 3 seconds

    lid.write(0);
    delay(500);   // Short delay after closing
  }

  delay(100);
}
```

## How to Run

1.  Assemble the circuit according to the connection tables.
2.  Install the Arduino IDE.
3.  Connect the Arduino Uno to your computer using a USB cable.
4.  Open the `.ino` sketch in Arduino IDE.
5.  Select **Arduino Uno** under **Tools → Board**.
6.  Select the correct port under **Tools → Port**.
7.  Upload the sketch to the board.
8.  Bring your hand or an object within approximately 20 cm of the
    ultrasonic sensor.
9.  The servo should rotate to 90 degrees, hold the lid open for 3
    seconds, and return to 0 degrees.

## Results

The project is designed to provide:

-   Touch-free lid opening.
-   Object detection at the configured 20 cm threshold.
-   Automatic lid closing after 3 seconds.
-   A low-cost build using a cardboard dustbin body.


## Project Demo Video

Watch the working demonstration of our Smart Dustbin:

[Click here to watch the Smart Dustbin Demo] https://drive.google.com/file/d/1hXXj4I3gHmxghmOc3mPjYeBpcet8kAZT/view?usp=drivesdk


## Applications

-   Homes and kitchens.
-   Hospitals and clinics.
-   Offices and schools.
-   Public places.

## Future Scope

Possible future improvements identified for the project include:

-   IoT-based dustbin fill-level alerts.
-   Solar-powered supply.
-   Wet and dry waste segregation.
-   Battery backup.

## Conclusion

The Smart Dustbin demonstrates a complete embedded-system loop ---
**sense, decide, and act** --- using an Arduino Uno, an ultrasonic
sensor, and a servo motor. It provides a simple, low-cost approach to
touch-free dustbin operation.

## Project Team

-   **Nitin Gupta** --- 202310101110122
-   **Prachi Chaurasia** --- 202310101110168
-   **Amrendra Maurya** --- 202310101110172

**Branch:** Computer Science (CS)\
**Project Guide:** Manju Bhardwaj\
**Institution:** Shri Ramswaroop Memorial University

------------------------------------------------------------------------

*Academic hardware project for the Microprocessor subject.*
