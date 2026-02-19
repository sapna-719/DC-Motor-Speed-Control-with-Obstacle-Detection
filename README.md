Arduino Code
// Motor pins
const int pwmPin = 5;  // PWM output (hardware PWM)
const int dir1   = 4;
const int dir2   = 7;
// Ultrasonic pins
const int trigPin = 10;
const int echoPin = 9;
// Stop range
const int MIN_STOP = 3;    // cm
const int MAX_STOP = 325;  // cm
void setup() {
  pinMode(pwmPin, OUTPUT);
  pinMode(dir1, OUTPUT);
  pinMode(dir2, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
  // Motor initial direction
  digitalWrite(dir1, HIGH);
  digitalWrite(dir2, LOW);
}
long readDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  long duration = pulseIn(echoPin, HIGH);
  long distance = duration * 0.034 / 2; // cm
  return distance;
}
void loop() {
  long dist = readDistance();
  Serial.print("Distance: ");
  Serial.print(dist);
  Serial.println(" cm");
  // Stop motor if distance is in the 3–325 cm range
  if (dist >= MIN_STOP && dist <= MAX_STOP) {
    analogWrite(pwmPin, 0); // stop
    Serial.println("Motor stopped");
  } else {
    analogWrite(pwmPin, 200); // run motor at fixed speed outside range
    Serial.println("Motor running");
  }
  delay(100); // 100ms loop
}
