# Ultrasonic Sensor Distance Measurement with Buzzer Alert

This Arduino project uses an ultrasonic sensor to measure distance and activate a buzzer if the distance falls below a specified threshold.

<img width="497" alt="Screenshot 2024-07-29 at 3 04 41 AM" src="https://github.com/user-attachments/assets/b75a3b28-9f2b-4dfd-bee3-f163b5ff558d">


## Components

- Arduino 
- Ultrasonic sensor (HC-SR04)
- Buzzer
- Breadboard and jumper wires

## Connections

### Ultrasonic Sensor (HC-SR04)

| Sensor Pin | Arduino Pin |
|------------|--------------|
| VCC        | 5V           |
| GND        | GND          |
| TRIG       | 9            |
| ECHO       | 10           |

### Buzzer

| Buzzer Pin | Arduino Pin |
|------------|--------------|
| +          | 11           |
| -          | GND          |

## Code

The provided code continuously measures the distance using the ultrasonic sensor and activates the buzzer if the measured distance is below the specified safety distance.


```cpp
// defines pins numbers
const int trigPin = 9; //used to trigger the ultrasonic sound pulses (output)
const int echoPin = 10; //produces a pulse when the reflected signal is received (input)
const int buzzer = 11;

// defines variables
long duration; //time taken for the ultrasonic pulse to return
int distance;
const int safetyDistance = 5; // Safety distance in cm

void setup() {
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin, INPUT); // Sets the echoPin as an Input
  pinMode(buzzer, OUTPUT);
  Serial.begin(9600); // Starts the serial communication
}

//loop to measure distance using the ultrasonic sensor and to activate a buzzer if the measured distance falls below a certain threshold
void loop() {
  // Clearing the trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2); //setting the trigPin to LOW and holding it there 2 microseconds

  // Sets the trigPin on HIGH state for 10 microseconds to send the ultrasonic pulse
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW); // Set trigPin LOW again after sending the pulse

  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH); //The pulseIn function reads the time it takes for the pulse to travel to the object and back, measuring the time the echoPin stays HIGH

  // Calculating the distance
  distance = duration * 0.034 / 2; //multiply the duration by 0.034 (speed of sound in cm per ms) to get the total distance traveled by the sound wave, divide by 2 to get the one-way distance

  // if the measured distance is less than or equal to the safety distance
  if (distance <= safetyDistance) {
    digitalWrite(buzzer, HIGH); // Activate buzzer
  } else {
    digitalWrite(buzzer, LOW); // Deactivate buzzer
  }

  // Prints the distance on the Serial Monitor
  Serial.print("Distance: ");
  Serial.println(distance);
  
  delay(100); // Short delay to avoid excessive serial output
}
