# Automatic-dustbin-project-


#include <Servo.h>

Servo lidServo;

int trigPin = 2;
int echoPin = 3;
int servoPin = 9;

long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  lidServo.attach(servoPin);
  lidServo.write(0); // Lid closed
  Serial.begin(9600);
}

void loop() {
  // Send ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.println(distance);

  if (distance > 0 && distance < 20) {  // Hand detected within 20cm
    lidServo.write(60); // Open lid to 60 degrees
    delay(3000);        // Keep open for 3 sec
    lidServo.write(0);  // Close lid
  }
}