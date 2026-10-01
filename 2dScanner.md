Here is my process for the simple scanner.
First, divide into two components, the stepper motor and the sensor. Thank you to Dr. Dzula for helping me get my supplies and giving me the idea of building the two parts of the scanner individually.
For the sensor, I plugged VCC into the 5 volt, Echo and Trig into their digital pins, and GND into ground on the arduino.
<img width="1440" height="1920" alt="image" src="https://github.com/user-attachments/assets/c8a6180d-c97a-44da-98f5-a0a32804c659" />
Then, for the motor drive, I connected all of the output holes to the digital pins and then since my arduino GND and 5V were almost filled, I connected a GND and 5v to the breadboard for more space. I connected the positive and negative on/off from the driver to the breadboard.
<img width="1440" height="1920" alt="image" src="https://github.com/user-attachments/assets/04a7b6d4-0a47-4905-bf1d-376f7b894044" />
And then for the code, I collected the code for the sensor from a tutorial on Arduino project hub and edited it to scan at faster intervals by changing the the delayMicroseconds. I then combined it with a tutorial script from Autodesk Instructables but changed the turn pattern by changing the steps to be from a full rotation to half a rotation by dividing stepsperrotation by 2. I also made it a bit faster by lowering the delay time at the end of each half rotation.
Unfortunately the hot glue fell off but the distance scanner works and the stepper motor works so if I hot glue them tomorrow, it'll be good.
A problem, though, could be with the intervals of the scanning since the scanner cannot just continuously scan.
<img width="1920" height="1440" alt="image" src="https://github.com/user-attachments/assets/1adb21ab-a444-4785-821e-9b4040e72751" />

My full code is this:

#include <Stepper.h>

//define Input pins of the Motor
#define OUTPUT1   7                
#define OUTPUT2   6                
#define OUTPUT3   5               
#define OUTPUT4   4               

// number of steps per rotation
const int stepsPerRotation = 1025;  // 28BYJ-48 has 2048 steps per rotation

Stepper myStepper(stepsPerRotation, OUTPUT1, OUTPUT3, OUTPUT2, OUTPUT4);  

const int trigPin = 9;
const int echoPin = 10;

float duration, distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
  // speed of the motor in RPM
  myStepper.setSpeed(10);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  // Turn halfway forward (180 degrees = half of full steps)
  myStepper.step(stepsPerRotation / 2);
  delay(500); //in milisecs

  // Turn halfway back (reverse direction by using a negative value)
  myStepper.step(-stepsPerRotation / 2);
  delay(500);

  digitalWrite(trigPin, LOW);
  delayMicroseconds(1);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(1);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration*.0343)/2;
  Serial.print("Distance: ");
  Serial.println(distance);
  delay(1);
}
