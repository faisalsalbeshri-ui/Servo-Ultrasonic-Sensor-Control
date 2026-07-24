# Servo Motor Control Using an Ultrasonic Sensor

This project demonstrates a practical Arduino system that controls a servo motor using an HC-SR04 ultrasonic distance sensor.

The system continuously measures the distance in front of the sensor. When an object is detected at a distance of 10 cm or less, the servo motor rotates to 90 degrees and the LED turns on. When the object moves farther than 10 cm, the servo motor returns to 0 degrees and the LED turns off.

## Project Objective

The main objective of this project is to understand how sensors and actuators can work together in an embedded system.

The project includes:

- Measuring distance using an ultrasonic sensor.
- Processing the measured distance using Arduino.
- Controlling a servo motor according to the distance.
- Using an LED as a visual indicator.
- Testing the circuit using real hardware components.

## Components Used

- Arduino Uno
- HC-SR04 Ultrasonic Sensor
- SG90 Servo Motor
- Green LED
- 220 Ω Resistor
- Breadboard
- Jumper Wires
- USB Cable

## Component Functions

### Arduino Uno

The Arduino Uno is the main controller of the project. It receives the distance measurement from the ultrasonic sensor and controls the servo motor and LED according to the programmed condition.

### HC-SR04 Ultrasonic Sensor

The HC-SR04 sensor measures the distance between the sensor and an object by sending ultrasonic waves and calculating the time required for the waves to return.

### SG90 Servo Motor

The servo motor moves to a specific angle based on the distance measured by the sensor.

- Object at 10 cm or less: Servo moves to 90°.
- Object farther than 10 cm: Servo returns to 0°.

### LED

The LED provides a visual indication when a nearby object is detected.

- LED ON: Object detected at 10 cm or less.
- LED OFF: Object is farther than 10 cm.

### 220 Ω Resistor

The resistor protects the LED by limiting the electrical current passing through it.

## Circuit Connections

### HC-SR04 Sensor Connections

| HC-SR04 Pin | Arduino Connection |
|---|---|
| VCC | 5V |
| GND | GND |
| TRIG | Digital Pin 9 |
| ECHO | Digital Pin 10 |

### Servo Motor Connections

| Servo Wire | Arduino Connection |
|---|---|
| Red | 5V |
| Brown | GND |
| Yellow / Orange | Digital Pin 3 |

### LED Connections

| LED Connection | Arduino Connection |
|---|---|
| Arduino D6 | 220 Ω Resistor |
| Resistor Output | LED Long Leg (+) |
| LED Short Leg (-) | GND |

## How the Project Works

1. The Arduino sends a short pulse through the TRIG pin of the HC-SR04 sensor.
2. The sensor sends ultrasonic waves toward the object.
3. The ECHO pin returns the time taken for the waves to travel to the object and back.
4. The Arduino calculates the distance in centimeters.
5. The calculated distance is compared with the activation distance of 10 cm.
6. If the distance is 10 cm or less:
   - The servo motor rotates to 90°.
   - The LED turns on.
7. If the distance is greater than 10 cm:
   - The servo motor returns to 0°.
   - The LED turns off.

## Arduino Code

```cpp
#include <Servo.h>

Servo myServo;

const int servoPin = 3;
const int ledPin = 6;
const int trigPin = 9;
const int echoPin = 10;

long duration;
float distance;

void setup()
{
  myServo.attach(servoPin);
  myServo.write(0);

  pinMode(ledPin, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  digitalWrite(ledPin, LOW);

  Serial.begin(9600);
}

void loop()
{
  // Send an ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);

  digitalWrite(trigPin, LOW);

  // Measure the echo duration
  duration = pulseIn(echoPin, HIGH, 30000);

  // Calculate the distance in centimeters
  if (duration == 0)
  {
    distance = 999;
  }
  else
  {
    distance = duration * 0.0343 / 2;
  }

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Control the servo motor and LED
  if (distance <= 10)
  {
    myServo.write(90);
    digitalWrite(ledPin, HIGH);
  }
  else
  {
    myServo.write(0);
    digitalWrite(ledPin, LOW);
  }

  delay(200);
}
